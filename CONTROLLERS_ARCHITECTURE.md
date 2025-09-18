# POS System Controllers Architecture & Function Mapping

## Table of Contents
1. [Controller Overview](#controller-overview)
2. [Core Controllers](#core-controllers)
3. [Business Management Controllers](#business-management-controllers)
4. [Product Management Controllers](#product-management-controllers)
5. [Transaction Controllers](#transaction-controllers)
6. [Financial Controllers](#financial-controllers)
7. [User & Access Controllers](#user--access-controllers)
8. [E-commerce Controllers](#e-commerce-controllers)
9. [Utility Controllers](#utility-controllers)
10. [Route Mapping](#route-mapping)
11. [Controller Patterns](#controller-patterns)

---

## Controller Overview

The POS system follows **Laravel MVC architecture** with 50+ controllers handling different aspects of business operations. Controllers are organized by functionality and follow REST principles where applicable.

### Controller Structure
```
app/Http/Controllers/
├── Core System Controllers
├── Business Management Controllers  
├── Product & Inventory Controllers
├── Transaction & Sales Controllers
├── Financial & Accounting Controllers
├── User Management Controllers
├── E-commerce Integration Controllers
├── Utility & Helper Controllers
└── API Controllers (separate namespace)
```

---

## Core Controllers

### 1. **Controller.php** (Base Controller)
**Purpose**: Base controller providing common functionality for all controllers

**Key Methods**:
- `getStatusCode()` - Get current HTTP status code
- `setStatusCode($code)` - Set HTTP status code
- `respondWithError($message)` - Return error response
- `respondUnauthorized($message)` - Return 403 unauthorized response
- `respondWentWrong($exception)` - Handle exceptions and return error response
- `respondSuccess($message, $data)` - Return success response
- `respond($data)` - Return JSON response
- `getMpdf()` - Create new mPDF instance for PDF generation

**Features**:
- ✅ Standardized API responses
- ✅ Exception handling
- ✅ PDF generation utilities
- ✅ RTL language support

---

### 2. **HomeController.php** (Dashboard)
**Purpose**: Main dashboard and application entry point

**Key Methods**:
- `index()` - Main dashboard view with charts, widgets, and statistics
- `getTotals()` - AJAX endpoint for financial totals (sales, purchases, expenses)
- `getProductStockAlert()` - Products with low stock alerts
- `getPurchasePaymentDues()` - Upcoming purchase payment dues
- `getSalesPaymentDues()` - Upcoming sales payment dues
- `loadMoreNotifications()` - Paginated notifications loading
- `getTotalUnreadNotifications()` - Count of unread notifications
- `getCalendar()` - Calendar view with bookings and events
- `showNotification($id)` - Display specific notification
- `attachMediasToGivenModel()` - File upload handler
- `getUserLocation($latlng)` - Get address from coordinates

**Features**:
- 📊 **Sales Charts**: Last 30 days and financial year sales trends
- 📈 **Financial Dashboard**: Revenue, expenses, and profit metrics
- 🔔 **Notification System**: Real-time notifications with popup modals
- 📅 **Calendar Integration**: Bookings and event management
- 📱 **Location Services**: GPS to address conversion
- 🏪 **Multi-location Support**: Location-specific dashboards

**Data Flow**:
```
Dashboard Request → HomeController@index → Fetch Data:
├── Sales Data (last 30 days + FY)
├── Purchase/Expense Totals  
├── Stock Alerts
├── Payment Dues
├── Module Widgets
└── Render Dashboard View
```

---

## Business Management Controllers

### 3. **BusinessController.php**
**Purpose**: Business registration, settings, and configuration

**Key Methods**:
- `getRegister()` - Business registration form
- `postRegister()` - Handle business registration
- `postCheckUsername()` - Username availability check
- `postCheckEmail()` - Email availability check
- `getBusinessSettings()` - Business settings form
- `postBusinessSettings()` - Update business settings
- `testEmailConfiguration()` - Test email settings
- `testSmsConfiguration()` - Test SMS settings

**Features**:
- 🏢 **Multi-tenant Registration**: Business signup with validation
- ⚙️ **Business Configuration**: Tax settings, accounting methods, currencies
- 📧 **Communication Settings**: Email and SMS configuration
- 🎨 **Theme Management**: UI customization options

### 4. **BusinessLocationController.php**
**Purpose**: Manage multiple business locations/stores

**Key Methods**:
- `index()` - List all locations
- `create()` - Add new location form
- `store()` - Save new location
- `show($id)` - View location details
- `edit($id)` - Edit location form
- `update($id)` - Update location
- `destroy($id)` - Delete location
- `toggleStatus($id)` - Activate/deactivate location

**Features**:
- 🏪 **Multi-location Management**: Unlimited business locations
- 📍 **Location Settings**: Address, contact, and operational details
- 🖨️ **Printer Configuration**: Receipt printers per location
- 💰 **Payment Settings**: Payment accounts per location

---

## Product Management Controllers

### 5. **ProductController.php**
**Purpose**: Core product catalog management

**Key Methods**:
- `index()` - Product listing with filters and pagination
- `create()` - Add product form
- `store()` - Save new product
- `show($id)` - Product details view
- `edit($id)` - Edit product form
- `update($id)` - Update product
- `destroy($id)` - Delete product
- `getProducts()` - AJAX product search for POS
- `getProductDetails($id)` - Detailed product info for variations
- `quickAdd()` - Quick product addition modal
- `massDestroy()` - Bulk product deletion
- `addSellingPrices($id)` - Manage selling price groups
- `savePricesFromListing()` - Quick price updates
- `toggleProductStatus($id)` - Activate/deactivate product

**Features**:
- 📦 **Product Variations**: Size, color, model variations
- 💰 **Pricing Management**: Multiple selling price groups
- 📊 **Stock Tracking**: Multi-location inventory
- 🏷️ **Barcode Generation**: Multiple barcode formats
- 📸 **Media Management**: Product images and documents
- 🔍 **Advanced Search**: Filter by category, brand, location
- 📈 **Product Analytics**: Sales history and performance

**Data Flow**:
```
Product Creation → ProductController@store → Process:
├── Validate Input Data
├── Create Product Record
├── Create Variations
├── Set Initial Stock
├── Generate Barcodes
├── Upload Media Files
└── Fire ProductCreated Event
```

### 6. **CategoryController.php** (TaxonomyController)
**Purpose**: Product categorization and hierarchy

**Key Methods**:
- `index()` - Category tree view
- `create()` - Add category form
- `store()` - Save new category
- `edit($id)` - Edit category
- `update($id)` - Update category
- `destroy($id)` - Delete category
- `getCategoriesDropdown()` - Category dropdown for forms

**Features**:
- 🌳 **Hierarchical Categories**: Parent-child relationships
- 🔗 **Sub-categories**: Unlimited nesting levels
- 📊 **Category Analytics**: Products per category

### 7. **BrandController.php**
**Purpose**: Product brand management

**Key Methods**:
- `index()` - Brand listing
- `create()` - Add brand form
- `store()` - Save new brand
- `edit($id)` - Edit brand
- `update($id)` - Update brand
- `destroy($id)` - Delete brand

**Features**:
- 🏷️ **Brand Management**: Product manufacturer/brand tracking
- 📈 **Brand Analytics**: Sales by brand

### 8. **UnitController.php**
**Purpose**: Measurement units management

**Key Methods**:
- `index()` - Units listing
- `create()` - Add unit form
- `store()` - Save new unit
- `edit($id)` - Edit unit
- `update($id)` - Update unit
- `destroy($id)` - Delete unit

**Features**:
- 📏 **Unit Management**: Weight, volume, length units
- 🔢 **Decimal Control**: Allow/disallow decimal quantities

### 9. **VariationTemplateController.php**
**Purpose**: Product variation templates (Size, Color templates)

**Key Methods**:
- `index()` - Template listing
- `create()` - Add template form
- `store()` - Save new template
- `edit($id)` - Edit template
- `update($id)` - Update template
- `destroy($id)` - Delete template

**Features**:
- 📋 **Reusable Templates**: Predefined variation sets
- 🎨 **Variation Types**: Size, color, model, etc.

---

## Transaction Controllers

### 10. **SellController.php**
**Purpose**: Sales transaction management

**Key Methods**:
- `index()` - Sales listing with filters
- `create()` - New sale form
- `store()` - Save new sale
- `show($id)` - Sale details view
- `edit($id)` - Edit sale form
- `update($id)` - Update sale
- `destroy($id)` - Delete sale
- `addSellReturn($id)` - Add return for sale
- `getSellDetails($id)` - AJAX sale details
- `printInvoice($id)` - Print sale invoice
- `duplicateSell($id)` - Duplicate existing sale

**Features**:
- 🛍️ **Sales Management**: Complete sales workflow
- 🧾 **Invoice Generation**: Customizable invoice layouts
- 💳 **Payment Processing**: Multiple payment methods
- 🔄 **Returns Processing**: Partial and full returns
- 💰 **Discounts**: Line-item and transaction-level discounts
- 📦 **Stock Updates**: Automatic inventory adjustments

### 11. **SellPosController.php**
**Purpose**: Point of Sale interface

**Key Methods**:
- `index()` - POS interface
- `create()` - New POS sale
- `store()` - Save POS sale
- `edit($id)` - Edit POS sale
- `update($id)` - Update POS sale
- `showInvoice($token)` - Public invoice view
- `invoicePayment($token)` - Online payment interface
- `confirmPayment($id)` - Confirm online payment
- `showServiceStaffAvailability()` - Service staff scheduler
- `pauseResumeServiceStaffTimer()` - Staff time tracking

**Features**:
- 💻 **Touch-friendly Interface**: Optimized for tablets
- 🛒 **Quick Product Selection**: Barcode scanning, search
- 💳 **Integrated Payments**: Cash, card, multiple methods
- 🧾 **Instant Receipts**: Print/email receipts
- 👥 **Customer Management**: Quick customer selection
- ⏱️ **Service Staff Timer**: Time tracking for services

### 12. **PurchaseController.php**
**Purpose**: Purchase transaction management

**Key Methods**:
- `index()` - Purchase listing
- `create()` - New purchase form
- `store()` - Save new purchase
- `show($id)` - Purchase details
- `edit($id)` - Edit purchase
- `update($id)` - Update purchase
- `destroy($id)` - Delete purchase
- `addPurchaseReturn($id)` - Process return
- `updatePurchaseStatus($id)` - Change purchase status

**Features**:
- 📥 **Purchase Management**: Complete procurement workflow
- 📦 **Stock Reception**: Receive partial/full orders
- 💰 **Cost Tracking**: Purchase prices and supplier management
- 🔄 **Purchase Returns**: Defective/excess item returns
- 📋 **Purchase Orders**: Order generation and tracking

### 13. **TransactionPaymentController.php**
**Purpose**: Payment management for all transactions

**Key Methods**:
- `addPayment($transaction_id)` - Add payment form
- `storePayment()` - Save payment
- `editPayment($payment_id)` - Edit payment
- `updatePayment($payment_id)` - Update payment
- `deletePayment($payment_id)` - Delete payment
- `getPaymentMethods()` - Available payment methods
- `printPaymentReceipt($payment_id)` - Print payment receipt

**Features**:
- 💳 **Multiple Payment Methods**: Cash, card, cheque, bank transfer
- 💰 **Partial Payments**: Split payments across methods
- 🧾 **Payment Receipts**: Customizable payment receipts
- 🔄 **Payment History**: Complete payment audit trail

---

## Financial Controllers

### 14. **AccountController.php**
**Purpose**: Chart of accounts management

**Key Methods**:
- `index()` - Accounts listing
- `create()` - Add account form
- `store()` - Save new account
- `show($id)` - Account details with transactions
- `edit($id)` - Edit account
- `update($id)` - Update account
- `closeAccount($id)` - Close/deactivate account

**Features**:
- 🏦 **Account Management**: Bank accounts, cash accounts
- 📊 **Account Balances**: Real-time balance calculations
- 📈 **Account Statements**: Transaction history

### 15. **ExpenseController.php**
**Purpose**: Business expense management

**Key Methods**:
- `index()` - Expense listing
- `create()` - Add expense form
- `store()` - Save new expense
- `show($id)` - Expense details
- `edit($id)` - Edit expense
- `update($id)` - Update expense
- `destroy($id)` - Delete expense

**Features**:
- 💸 **Expense Tracking**: Business expense management
- 📂 **Expense Categories**: Organized expense classification
- 🧾 **Expense Receipts**: Document attachments

### 16. **TaxRateController.php**
**Purpose**: Tax configuration and management

**Key Methods**:
- `index()` - Tax rates listing
- `create()` - Add tax rate form
- `store()` - Save new tax rate
- `edit($id)` - Edit tax rate
- `update($id)` - Update tax rate
- `destroy($id)` - Delete tax rate

**Features**:
- 💰 **Tax Management**: Multiple tax rates
- 🏷️ **Tax Groups**: Composite tax calculations
- 📊 **Tax Reports**: Tax collection summaries

### 17. **ReportController.php**
**Purpose**: Business reports and analytics

**Key Methods**:
- `profitLoss()` - Profit & Loss report
- `stockReport()` - Inventory/stock report
- `purchaseReport()` - Purchase analysis
- `sellReport()` - Sales analysis
- `contactReport()` - Customer/supplier reports
- `taxReport()` - Tax collection reports
- `expenseReport()` - Expense analysis
- `registerReport()` - Cash register reports

**Features**:
- 📈 **Financial Reports**: P&L, balance sheet
- 📊 **Sales Analytics**: Trends, top products
- 📦 **Inventory Reports**: Stock levels, movements
- 👥 **Customer Analytics**: Purchase patterns
- 💰 **Tax Reports**: Government compliance

---

## User & Access Controllers

### 18. **UserController.php**
**Purpose**: User profile management

**Key Methods**:
- `getProfile()` - User profile view
- `updateProfile()` - Update profile information
- `updatePassword()` - Change password

**Features**:
- 👤 **Profile Management**: Personal information updates
- 🔐 **Password Management**: Secure password changes
- 🌐 **Language Settings**: Multi-language support

### 19. **ManageUserController.php**
**Purpose**: User administration (Admin only)

**Key Methods**:
- `index()` - Users listing
- `create()` - Add user form
- `store()` - Save new user
- `show($id)` - User details
- `edit($id)` - Edit user
- `update($id)` - Update user
- `destroy($id)` - Delete user
- `signInAsUser($id)` - Impersonate user (Superadmin)

**Features**:
- 👥 **User Management**: Complete user administration
- 🔑 **Role Assignment**: User permissions and roles
- 🏪 **Location Access**: User location permissions
- 👨‍💼 **User Impersonation**: Support and debugging

### 20. **RoleController.php**
**Purpose**: Role and permission management

**Key Methods**:
- `index()` - Roles listing
- `create()` - Add role form
- `store()` - Save new role
- `edit($id)` - Edit role
- `update($id)` - Update role
- `destroy($id)` - Delete role

**Features**:
- 🔐 **Permission System**: Granular access control
- 👑 **Role Management**: Custom roles per business
- 🛡️ **Security**: Business-scoped permissions

---

## E-commerce Controllers

### 21. **CarouselImageController.php**
**Purpose**: Homepage carousel management

**Key Methods**:
- `index()` - Carousel images listing
- `create()` - Add image form
- `store()` - Save new image
- `edit($id)` - Edit image
- `update($id)` - Update image
- `destroy($id)` - Delete image
- `toggleStatus($id)` - Show/hide image

**Features**:
- 🖼️ **Image Management**: Homepage carousel images
- 📱 **Responsive Images**: Mobile-optimized display
- 🎯 **Marketing Tool**: Promotional image display

### 22. **YouTubeVideoController.php**
**Purpose**: Video content management

**Key Methods**:
- `index()` - Videos listing
- `create()` - Add video form
- `store()` - Save new video
- `edit($id)` - Edit video
- `update($id)` - Update video
- `destroy($id)` - Delete video
- `toggleStatus($id)` - Show/hide video

**Features**:
- 📹 **Video Integration**: YouTube video embedding
- 🎬 **Content Management**: Product demonstration videos
- 📱 **Marketing Content**: Promotional videos

### 23. **EcommerceSyncController.php**
**Purpose**: E-commerce platform synchronization

**Key Methods**:
- `index()` - Product sync dashboard
- `syncProduct($id)` - Sync single product
- `unsyncProduct($id)` - Remove product from sync
- `bulkSync()` - Bulk product synchronization
- `bulkUnsync()` - Bulk removal from sync

**Features**:
- 🔄 **Product Sync**: E-commerce platform integration
- 📦 **Inventory Sync**: Stock level synchronization
- 💰 **Price Sync**: Pricing updates across platforms
- 🛒 **Multi-platform**: Support multiple e-commerce sites

### 24. **FrontendOrderController.php**
**Purpose**: Online order management

**Key Methods**:
- `index()` - Orders listing
- `show($id)` - Order details
- `update($id)` - Update order status
- `accept($id)` - Accept order
- `complete($id)` - Mark order complete
- `cancel($id)` - Cancel order

**Features**:
- 📱 **Online Orders**: E-commerce order processing
- 📦 **Order Management**: Status tracking and updates
- 🚚 **Fulfillment**: Order completion workflow
- 💳 **Payment Integration**: Online payment processing

---

## Utility Controllers

### 25. **CashRegisterController.php**
**Purpose**: Cash register/till management

**Key Methods**:
- `index()` - Register sessions listing
- `create()` - Open register form
- `store()` - Open new register session
- `show($id)` - Register session details
- `edit($id)` - Close register form
- `update($id)` - Close register session
- `getCurrentCashRegister()` - Get active register
- `cashIn($register_id)` - Add cash to register
- `cashOut($register_id)` - Remove cash from register

**Features**:
- 💰 **Cash Management**: Register opening/closing
- 📊 **Session Tracking**: Sales and payments per session
- 💸 **Cash Flow**: Cash in/out operations
- 🧾 **Register Reports**: Session summaries

### 26. **StockAdjustmentController.php**
**Purpose**: Inventory adjustments

**Key Methods**:
- `index()` - Adjustments listing
- `create()` - New adjustment form
- `store()` - Save adjustment
- `show($id)` - Adjustment details
- `edit($id)` - Edit adjustment
- `update($id)` - Update adjustment
- `destroy($id)` - Delete adjustment

**Features**:
- 📦 **Stock Adjustments**: Inventory corrections
- 🔍 **Adjustment Tracking**: Audit trail for changes
- 📊 **Adjustment Reports**: Stock movement summaries

### 27. **StockTransferController.php**
**Purpose**: Inter-location stock transfers

**Key Methods**:
- `index()` - Transfers listing
- `create()` - New transfer form
- `store()` - Save transfer
- `show($id)` - Transfer details
- `edit($id)` - Edit transfer
- `update($id)` - Update transfer
- `destroy($id)` - Delete transfer

**Features**:
- 🚚 **Stock Transfers**: Move inventory between locations
- 📦 **Transfer Tracking**: In-transit inventory management
- 📊 **Transfer Reports**: Movement summaries

### 28. **BarcodeController.php**
**Purpose**: Barcode generation and printing

**Key Methods**:
- `index()` - Barcode generation interface
- `preview()` - Barcode preview
- `generate()` - Generate barcodes
- `print()` - Print barcode labels

**Features**:
- 🏷️ **Barcode Generation**: Multiple formats (EAN, UPC, Code128)
- 🖨️ **Label Printing**: Customizable label layouts
- 📊 **Batch Generation**: Bulk barcode creation

### 29. **ImportController.php** (Multiple Import Controllers)
**Purpose**: Data import functionality

**Controllers**:
- `ImportProductsController` - Product data import
- `ImportOpeningStockController` - Opening stock import
- `ImportSalesController` - Sales data import

**Features**:
- 📥 **CSV Import**: Bulk data import from CSV files
- ✅ **Data Validation**: Import error checking
- 📊 **Import Reports**: Success/failure summaries

---

## Route Mapping

### Route Structure
```php
// Public Routes (Guest)
Route::get('/', redirect to login)
Route::get('/business/register', 'BusinessController@getRegister')
Route::get('/invoice/{token}', 'SellPosController@showInvoice')

// Authenticated Routes
Route::middleware(['auth', 'SetSessionData'])->group(function () {
    // Dashboard
    Route::get('/home', 'HomeController@index')
    
    // Resource Routes (CRUD)
    Route::resource('products', 'ProductController')
    Route::resource('brands', 'BrandController')
    Route::resource('categories', 'TaxonomyController')
    Route::resource('contacts', 'ContactController')
    Route::resource('purchases', 'PurchaseController')
    Route::resource('sells', 'SellController')
    
    // Custom Routes
    Route::get('pos/create', 'SellPosController@create')
    Route::post('pos', 'SellPosController@store')
    Route::get('products/list', 'ProductController@getProducts')
});
```

### Route Patterns

#### 1. **Standard REST Routes**
```php
GET    /products           ProductController@index     (list)
GET    /products/create    ProductController@create    (form)
POST   /products           ProductController@store     (save)
GET    /products/{id}      ProductController@show      (view)
GET    /products/{id}/edit ProductController@edit      (edit form)
PUT    /products/{id}      ProductController@update    (update)
DELETE /products/{id}      ProductController@destroy   (delete)
```

#### 2. **AJAX/API Routes**
```php
GET    /home/get-totals                HomeController@getTotals
GET    /products/list                  ProductController@getProducts
POST   /purchases/get-purchase-entry   PurchaseController@getPurchaseEntry
GET    /sells/{id}/print               SellController@printInvoice
```

#### 3. **Custom Action Routes**
```php
GET    /cash-register/close/{id}       CashRegisterController@close
POST   /stock-adjustment/get-lot       StockAdjustmentController@getLot
PUT    /products/{id}/toggle-status    ProductController@toggleStatus
```

---

## Controller Patterns

### 1. **Standard CRUD Pattern**
```php
class ProductController extends Controller {
    public function index()     // List with filters & pagination
    public function create()    // Form view
    public function store()     // Save with validation
    public function show($id)   // Detail view
    public function edit($id)   // Edit form
    public function update($id) // Update with validation
    public function destroy($id)// Soft delete
}
```

### 2. **API Response Pattern**
```php
// Success Response
return $this->respondSuccess('Product created successfully', [
    'product_id' => $product->id
]);

// Error Response
return $this->respondWithError('Product not found');

// Exception Handling
return $this->respondWentWrong($exception);
```

### 3. **DataTables Pattern**
```php
public function index() {
    if (request()->ajax()) {
        $query = Product::with(['brand', 'category']);
        return DataTables::of($query)
            ->addColumn('action', function($row) {
                return view('product.actions', compact('row'));
            })
            ->editColumn('price', function($row) {
                return '<span class="display_currency">' . $row->price . '</span>';
            })
            ->rawColumns(['action', 'price'])
            ->make(true);
    }
    return view('product.index');
}
```

### 4. **Business Scope Pattern**
```php
public function index() {
    $business_id = request()->session()->get('user.business_id');
    $products = Product::where('business_id', $business_id)->get();
    return view('product.index', compact('products'));
}
```

### 5. **Permission Check Pattern**
```php
public function create() {
    if (!auth()->user()->can('product.create')) {
        abort(403, 'Unauthorized action.');
    }
    return view('product.create');
}
```

### 6. **Transaction Pattern**
```php
public function store(Request $request) {
    DB::beginTransaction();
    try {
        $product = Product::create($request->validated());
        $this->createVariations($product, $request->variations);
        $this->updateStock($product, $request->stock);
        
        DB::commit();
        return $this->respondSuccess('Product created');
    } catch (Exception $e) {
        DB::rollBack();
        return $this->respondWentWrong($e);
    }
}
```

---

## Controller Dependencies & Utils

### Utility Classes Used
- **ProductUtil**: Product-related business logic
- **BusinessUtil**: Business operations and calculations
- **TransactionUtil**: Transaction processing logic
- **ModuleUtil**: Module system integration
- **RestaurantUtil**: Restaurant-specific operations

### Common Dependencies
```php
// In Controller Constructor
public function __construct(
    ProductUtil $productUtil,
    BusinessUtil $businessUtil,
    TransactionUtil $transactionUtil
) {
    $this->productUtil = $productUtil;
    $this->businessUtil = $businessUtil;
    $this->transactionUtil = $transactionUtil;
}
```

### Middleware Stack
- `setData` - Set application data
- `auth` - Authentication required
- `SetSessionData` - Business session data
- `language` - Multi-language support
- `timezone` - Timezone handling
- `AdminSidebarMenu` - Navigation menu
- `CheckUserLogin` - Additional user checks

This comprehensive controller architecture provides a robust foundation for the POS system, handling everything from basic CRUD operations to complex business workflows while maintaining clean separation of concerns and reusable patterns.
