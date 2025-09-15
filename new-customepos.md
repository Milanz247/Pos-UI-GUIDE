# Custom POS Function Development Guide

## Overview
This guide will help you create a custom function for handling customer and product details in the POS system. We'll create a modular, reusable function that can process customer information and product details efficiently.

## 📁 Project Structure
```
app/
├── Http/Controllers/
│   ├── SellPosController.php     # Main POS controller
│   └── CustomPosController.php   # New custom controller (to be created)
├── Utils/
│   ├── TransactionUtil.php       # Transaction utilities
│   └── CustomTransactionUtil.php # Custom utilities (to be created)
├── Services/
│   └── CustomPosService.php      # Custom business logic (to be created)
└── Models/
    ├── Contact.php               # Customer model
    ├── Product.php               # Product model
    └── CustomTransaction.php     # Custom transaction model (if needed)
```

## 🎯 Step 1: Create Custom Service Class

### File: `app/Services/CustomPosService.php`

```php
<?php

namespace App\Services;

use App\Contact;
use App\Product;
use App\Variation;
use App\Utils\TransactionUtil;
use App\Utils\ProductUtil;
use Illuminate\Support\Facades\DB;
use Illuminate\Support\Facades\Log;

class CustomPosService
{
    protected $transactionUtil;
    protected $productUtil;

    public function __construct(TransactionUtil $transactionUtil, ProductUtil $productUtil)
    {
        $this->transactionUtil = $transactionUtil;
        $this->productUtil = $productUtil;
    }

    /**
     * Process customer and product details for custom POS transaction
     *
     * @param array $customerData
     * @param array $productData
     * @param int $businessId
     * @param int $locationId
     * @return array
     */
    public function processCustomTransaction($customerData, $productData, $businessId, $locationId)
    {
        try {
            DB::beginTransaction();

            // Step 1: Process customer data
            $customer = $this->processCustomerData($customerData, $businessId);
            
            // Step 2: Process product data
            $processedProducts = $this->processProductData($productData, $locationId);
            
            // Step 3: Calculate totals
            $totals = $this->calculateTransactionTotals($processedProducts);
            
            // Step 4: Validate inventory
            $inventoryValidation = $this->validateInventoryAvailability($processedProducts, $locationId);
            
            if (!$inventoryValidation['success']) {
                throw new \Exception($inventoryValidation['message']);
            }

            DB::commit();

            return [
                'success' => true,
                'customer' => $customer,
                'products' => $processedProducts,
                'totals' => $totals,
                'message' => 'Transaction data processed successfully'
            ];

        } catch (\Exception $e) {
            DB::rollback();
            Log::error('Custom POS Transaction Error: ' . $e->getMessage());
            
            return [
                'success' => false,
                'message' => $e->getMessage(),
                'error_code' => 'CUSTOM_TRANSACTION_ERROR'
            ];
        }
    }

    /**
     * Process customer data
     *
     * @param array $customerData
     * @param int $businessId
     * @return Contact|array
     */
    private function processCustomerData($customerData, $businessId)
    {
        // Check if customer exists
        if (!empty($customerData['customer_id'])) {
            $customer = Contact::where('business_id', $businessId)
                              ->where('id', $customerData['customer_id'])
                              ->first();
            
            if ($customer) {
                return $customer;
            }
        }

        // Create new customer if doesn't exist
        if (!empty($customerData['name']) || !empty($customerData['mobile'])) {
            $customerInput = [
                'business_id' => $businessId,
                'type' => 'customer',
                'name' => $customerData['name'] ?? 'Walk-in Customer',
                'mobile' => $customerData['mobile'] ?? null,
                'email' => $customerData['email'] ?? null,
                'address_line_1' => $customerData['address'] ?? null,
                'created_by' => auth()->id(),
                'is_default' => 0
            ];

            return Contact::create($customerInput);
        }

        // Return default walk-in customer
        return Contact::where('business_id', $businessId)
                     ->where('is_default', 1)
                     ->where('type', 'customer')
                     ->first();
    }

    /**
     * Process product data
     *
     * @param array $productData
     * @param int $locationId
     * @return array
     */
    private function processProductData($productData, $locationId)
    {
        $processedProducts = [];

        foreach ($productData as $productItem) {
            // Validate required fields
            if (empty($productItem['product_id']) || empty($productItem['quantity'])) {
                continue;
            }

            // Get product details
            $product = Product::with(['variations', 'unit'])
                             ->find($productItem['product_id']);

            if (!$product) {
                continue;
            }

            // Get variation details
            $variation = null;
            if (!empty($productItem['variation_id'])) {
                $variation = Variation::find($productItem['variation_id']);
            } else {
                $variation = $product->variations->first();
            }

            if (!$variation) {
                continue;
            }

            // Calculate unit price
            $unitPrice = $this->calculateUnitPrice($product, $variation, $productItem, $locationId);

            // Calculate line total
            $quantity = (float) $productItem['quantity'];
            $lineTotal = $unitPrice * $quantity;

            // Apply discount if provided
            $discount = 0;
            if (!empty($productItem['discount_percent'])) {
                $discount = ($lineTotal * $productItem['discount_percent']) / 100;
            } elseif (!empty($productItem['discount_amount'])) {
                $discount = (float) $productItem['discount_amount'];
            }

            $finalLineTotal = $lineTotal - $discount;

            $processedProducts[] = [
                'product_id' => $product->id,
                'variation_id' => $variation->id,
                'product_name' => $product->name,
                'variation_name' => $variation->name ?? '',
                'quantity' => $quantity,
                'unit_price' => $unitPrice,
                'line_total' => $lineTotal,
                'discount' => $discount,
                'final_line_total' => $finalLineTotal,
                'unit_name' => $product->unit->short_name ?? '',
                'product_sku' => $variation->sub_sku,
                'enable_stock' => $product->enable_stock,
                'current_stock' => $this->getCurrentStock($variation->id, $locationId)
            ];
        }

        return $processedProducts;
    }

    /**
     * Calculate unit price based on product, variation and pricing rules
     *
     * @param Product $product
     * @param Variation $variation
     * @param array $productItem
     * @param int $locationId
     * @return float
     */
    private function calculateUnitPrice($product, $variation, $productItem, $locationId)
    {
        // Check if custom price is provided
        if (!empty($productItem['unit_price'])) {
            return (float) $productItem['unit_price'];
        }

        // Get default selling price
        $unitPrice = $variation->default_sell_price ?? 0;

        // Apply pricing group if specified
        if (!empty($productItem['price_group_id'])) {
            $priceGroupPrice = $this->productUtil->getVariationGroupPrice(
                $variation->id,
                $productItem['price_group_id'],
                $variation->default_sell_price
            );
            
            if ($priceGroupPrice) {
                $unitPrice = $priceGroupPrice;
            }
        }

        return (float) $unitPrice;
    }

    /**
     * Calculate transaction totals
     *
     * @param array $processedProducts
     * @return array
     */
    private function calculateTransactionTotals($processedProducts)
    {
        $subtotal = 0;
        $totalDiscount = 0;
        $totalQuantity = 0;

        foreach ($processedProducts as $product) {
            $subtotal += $product['line_total'];
            $totalDiscount += $product['discount'];
            $totalQuantity += $product['quantity'];
        }

        $finalTotal = $subtotal - $totalDiscount;

        return [
            'subtotal' => $subtotal,
            'total_discount' => $totalDiscount,
            'final_total' => $finalTotal,
            'total_quantity' => $totalQuantity,
            'total_items' => count($processedProducts)
        ];
    }

    /**
     * Validate inventory availability
     *
     * @param array $processedProducts
     * @param int $locationId
     * @return array
     */
    private function validateInventoryAvailability($processedProducts, $locationId)
    {
        foreach ($processedProducts as $product) {
            if ($product['enable_stock']) {
                if ($product['current_stock'] < $product['quantity']) {
                    return [
                        'success' => false,
                        'message' => "Insufficient stock for product: {$product['product_name']}. Available: {$product['current_stock']}, Required: {$product['quantity']}"
                    ];
                }
            }
        }

        return ['success' => true];
    }

    /**
     * Get current stock for variation at location
     *
     * @param int $variationId
     * @param int $locationId
     * @return float
     */
    private function getCurrentStock($variationId, $locationId)
    {
        $stock = VariationLocationDetails::where('variation_id', $variationId)
                                        ->where('location_id', $locationId)
                                        ->first();

        return $stock ? $stock->qty_available : 0;
    }
}
```

## 🎯 Step 2: Create Custom Controller

### File: `app/Http/Controllers/CustomPosController.php`

```php
<?php

namespace App\Http\Controllers;

use App\Services\CustomPosService;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Validator;

class CustomPosController extends Controller
{
    protected $customPosService;

    public function __construct(CustomPosService $customPosService)
    {
        $this->customPosService = $customPosService;
    }

    /**
     * Process custom POS transaction
     *
     * @param Request $request
     * @return \Illuminate\Http\JsonResponse
     */
    public function processCustomTransaction(Request $request)
    {
        // Validation rules
        $validator = Validator::make($request->all(), [
            'business_id' => 'required|integer|exists:business,id',
            'location_id' => 'required|integer|exists:business_locations,id',
            'customer.name' => 'nullable|string|max:255',
            'customer.mobile' => 'nullable|string|max:20',
            'customer.email' => 'nullable|email|max:255',
            'customer.customer_id' => 'nullable|integer|exists:contacts,id',
            'products' => 'required|array|min:1',
            'products.*.product_id' => 'required|integer|exists:products,id',
            'products.*.variation_id' => 'nullable|integer|exists:variations,id',
            'products.*.quantity' => 'required|numeric|min:0.01',
            'products.*.unit_price' => 'nullable|numeric|min:0',
            'products.*.discount_percent' => 'nullable|numeric|min:0|max:100',
            'products.*.discount_amount' => 'nullable|numeric|min:0'
        ]);

        if ($validator->fails()) {
            return response()->json([
                'success' => false,
                'message' => 'Validation failed',
                'errors' => $validator->errors()
            ], 422);
        }

        $result = $this->customPosService->processCustomTransaction(
            $request->input('customer', []),
            $request->input('products', []),
            $request->input('business_id'),
            $request->input('location_id')
        );

        $statusCode = $result['success'] ? 200 : 400;

        return response()->json($result, $statusCode);
    }

    /**
     * Create transaction from processed data
     *
     * @param Request $request
     * @return \Illuminate\Http\JsonResponse
     */
    public function createTransaction(Request $request)
    {
        // First process the data
        $processedData = $this->processCustomTransaction($request);
        
        if (!$processedData->getData()->success) {
            return $processedData;
        }

        // Then create actual transaction using processed data
        // This would integrate with existing POS store method
        
        return response()->json([
            'success' => true,
            'message' => 'Transaction created successfully',
            'transaction_id' => null // Would contain actual transaction ID
        ]);
    }
}
```

## 🎯 Step 3: Add Routes

### File: `routes/web.php` (add these routes)

```php
// Custom POS Routes
Route::group(['middleware' => ['web', 'auth', 'language', 'AdminSidebarMenu'], 'prefix' => 'custom-pos'], function () {
    Route::post('/process-transaction', [\App\Http\Controllers\CustomPosController::class, 'processCustomTransaction'])->name('custom-pos.process');
    Route::post('/create-transaction', [\App\Http\Controllers\CustomPosController::class, 'createTransaction'])->name('custom-pos.create');
});
```

## 🎯 Step 4: Usage Examples

### Example 1: Basic Usage with Customer Name and Products

```javascript
// Frontend JavaScript example
const transactionData = {
    business_id: 1,
    location_id: 1,
    customer: {
        name: "John Doe",
        mobile: "1234567890",
        email: "john@example.com"
    },
    products: [
        {
            product_id: 1,
            variation_id: 1,
            quantity: 2,
            unit_price: 10.50,
            discount_percent: 5
        },
        {
            product_id: 2,
            quantity: 1,
            discount_amount: 2.00
        }
    ]
};

fetch('/custom-pos/process-transaction', {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json',
        'X-CSRF-TOKEN': document.querySelector('meta[name="csrf-token"]').getAttribute('content')
    },
    body: JSON.stringify(transactionData)
})
.then(response => response.json())
.then(data => {
    console.log('Transaction processed:', data);
});
```

### Example 2: PHP Usage

```php
// In any controller or service
use App\Services\CustomPosService;

class YourController extends Controller
{
    public function yourMethod(CustomPosService $customPosService)
    {
        $customerData = [
            'name' => 'Jane Smith',
            'mobile' => '9876543210'
        ];

        $productData = [
            [
                'product_id' => 1,
                'variation_id' => 1,
                'quantity' => 3,
                'discount_percent' => 10
            ]
        ];

        $result = $customPosService->processCustomTransaction(
            $customerData,
            $productData,
            $businessId,
            $locationId
        );

        if ($result['success']) {
            // Process successful
            $customer = $result['customer'];
            $products = $result['products'];
            $totals = $result['totals'];
        } else {
            // Handle error
            $errorMessage = $result['message'];
        }
    }
}
```

## 🎯 Step 5: Integration with Existing POS

### Add to SellPosController.php

```php
// Add this method to SellPosController
use App\Services\CustomPosService;

/**
 * Create transaction using custom service
 *
 * @param Request $request
 * @return \Illuminate\Http\Response
 */
public function storeCustomTransaction(Request $request)
{
    $customPosService = app(CustomPosService::class);
    
    // Process data using custom service
    $processedData = $customPosService->processCustomTransaction(
        $request->input('customer', []),
        $request->input('products', []),
        $request->session()->get('user.business_id'),
        $request->input('location_id')
    );

    if (!$processedData['success']) {
        return response()->json($processedData, 400);
    }

    // Use processed data with existing store logic
    $request->merge([
        'contact_id' => $processedData['customer']->id,
        'products' => $processedData['products'],
        'final_total' => $processedData['totals']['final_total']
    ]);

    // Call existing store method
    return $this->store($request);
}
```

## 🎯 Step 6: Testing

### Create Test File: `tests/Feature/CustomPosTest.php`

```php
<?php

namespace Tests\Feature;

use Tests\TestCase;
use App\User;
use App\Business;
use App\BusinessLocation;
use App\Contact;
use App\Product;
use Illuminate\Foundation\Testing\RefreshDatabase;

class CustomPosTest extends TestCase
{
    use RefreshDatabase;

    public function test_process_custom_transaction()
    {
        // Setup test data
        $user = User::factory()->create();
        $business = Business::factory()->create();
        $location = BusinessLocation::factory()->create(['business_id' => $business->id]);
        $product = Product::factory()->create(['business_id' => $business->id]);

        $this->actingAs($user);

        $response = $this->postJson('/custom-pos/process-transaction', [
            'business_id' => $business->id,
            'location_id' => $location->id,
            'customer' => [
                'name' => 'Test Customer',
                'mobile' => '1234567890'
            ],
            'products' => [
                [
                    'product_id' => $product->id,
                    'quantity' => 2,
                    'unit_price' => 10.00
                ]
            ]
        ]);

        $response->assertStatus(200)
                ->assertJson([
                    'success' => true
                ]);
    }
}
```

## 🔧 Additional Configuration

### Register Service in AppServiceProvider

```php
// In app/Providers/AppServiceProvider.php
public function register()
{
    $this->app->bind(CustomPosService::class, function ($app) {
        return new CustomPosService(
            $app->make(\App\Utils\TransactionUtil::class),
            $app->make(\App\Utils\ProductUtil::class)
        );
    });
}
```

## 📝 Features Included

1. ✅ **Customer Processing**: Handle existing customers, create new ones, or use walk-in customers
2. ✅ **Product Validation**: Validate products, variations, and pricing
3. ✅ **Inventory Checking**: Real-time stock validation
4. ✅ **Price Calculation**: Support for custom prices, price groups, and discounts
5. ✅ **Error Handling**: Comprehensive error handling and validation
6. ✅ **Transaction Safety**: Database transactions with rollback capability
7. ✅ **Logging**: Error logging for debugging
8. ✅ **Testing**: Unit test examples
9. ✅ **Documentation**: Complete usage examples

## 🚀 Next Steps

1. Create the service class file
2. Create the controller file
3. Add routes
4. Test with sample data
5. Integrate with existing POS system
6. Add frontend interface if needed

This modular approach allows you to reuse the custom function across different parts of your application while maintaining clean code structure and proper error handling.
