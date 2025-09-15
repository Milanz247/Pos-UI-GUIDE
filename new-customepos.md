# Frontend Orders API Documentation

## API Endpoint: Create Order

**URL:** `POST /api/orders`  
**Controller:** `FrontendOrderController@apiStore`  
**Authentication:** Not required (public API)  
**Content-Type:** `application/json`

---

## Request Format

### Required Headers
```http
Content-Type: application/json
```

### Request Body Structure
```json
{
  "customer": {
    "name": "string (required, max 255 chars)",
    "phone": "string (required, max 20 chars)",
    "address": "string (required)",
    "nearest_city": "string (required, max 100 chars)",
    "district": "string (required, max 100 chars)",
    "notes": "string (optional)"
  },
  "items": [
    {
      "product_id": "integer (required, must exist in products table)",
      "variation_id": "integer (optional, must exist in variations table if provided)",
      "quantity": "integer (required, minimum 1)"
    }
  ]
}
```

### Example Request
```json
{
  "customer": {
    "name": "John Doe",
    "phone": "+94771234567",
    "address": "123 Main Street, Colombo 03",
    "nearest_city": "Colombo",
    "district": "Western",
    "notes": "Please call before delivery"
  },
  "items": [
    {
      "product_id": 1,
      "variation_id": 5,
      "quantity": 2
    },
    {
      "product_id": 3,
      "variation_id": null,
      "quantity": 1
    }
  ]
}
```

---

## Response Formats

### Success Response (HTTP 201)
```json
{
  "success": true,
  "message": "Order placed successfully",
  "order_number": "ORD-20250915-0001",
  "order_id": 123,
  "calculated_totals": {
    "subtotal": 150.00,
    "tax": 0.00,
    "shipping": 0.00,
    "total": 150.00
  }
}
```

### Validation Error Response (HTTP 422)
```json
{
  "success": false,
  "message": "Validation failed",
  "errors": {
    "customer.name": [
      "The customer name field is required."
    ],
    "items.0.product_id": [
      "The selected product id is invalid."
    ],
    "items.1.quantity": [
      "The quantity must be at least 1."
    ]
  }
}
```

### Server Error Response (HTTP 500)
```json
{
  "success": false,
  "message": "Order creation failed"
}
```

---

## Implementation Examples

### JavaScript/Next.js Example
```javascript
async function createOrder(orderData) {
  try {
    const response = await fetch('/api/orders', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(orderData)
    });

    const result = await response.json();

    if (response.ok && result.success) {
      console.log('Order created:', result.order_number);
      console.log('Order ID:', result.order_id);
      console.log('Total:', result.calculated_totals.total);
      return result;
    } else {
      console.error('Order creation failed:', result.message);
      if (result.errors) {
        console.error('Validation errors:', result.errors);
      }
      throw new Error(result.message);
    }
  } catch (error) {
    console.error('Network error:', error);
    throw error;
  }
}

// Usage example
const orderData = {
  customer: {
    name: "Jane Smith",
    phone: "+94712345678",
    address: "456 Oak Street, Kandy",
    nearest_city: "Kandy",
    district: "Central",
    notes: "Rush delivery needed"
  },
  items: [
    {
      product_id: 1,
      variation_id: 2,
      quantity: 1
    }
  ]
};

createOrder(orderData)
  .then(result => {
    // Handle success
    alert(`Order ${result.order_number} created successfully!`);
  })
  .catch(error => {
    // Handle error
    alert(`Error: ${error.message}`);
  });
```

### cURL Example
```bash
curl -X POST http://your-domain.com/api/orders \
  -H "Content-Type: application/json" \
  -d '{
    "customer": {
      "name": "Test Customer",
      "phone": "+94771234567",
      "address": "Test Address, Colombo",
      "nearest_city": "Colombo",
      "district": "Western",
      "notes": "Test order"
    },
    "items": [
      {
        "product_id": 1,
        "variation_id": 1,
        "quantity": 2
      }
    ]
  }'
```

### PHP Example
```php
<?php
$orderData = [
    'customer' => [
        'name' => 'PHP Test Customer',
        'phone' => '+94771234567',
        'address' => 'PHP Test Address',
        'nearest_city' => 'Galle',
        'district' => 'Southern',
        'notes' => 'PHP API test'
    ],
    'items' => [
        [
            'product_id' => 1,
            'variation_id' => 1,
            'quantity' => 1
        ]
    ]
];

$ch = curl_init();
curl_setopt($ch, CURLOPT_URL, 'http://your-domain.com/api/orders');
curl_setopt($ch, CURLOPT_POST, 1);
curl_setopt($ch, CURLOPT_POSTFIELDS, json_encode($orderData));
curl_setopt($ch, CURLOPT_HTTPHEADER, [
    'Content-Type: application/json',
]);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);

$response = curl_exec($ch);
$httpCode = curl_getinfo($ch, CURLINFO_HTTP_CODE);
curl_close($ch);

$result = json_decode($response, true);

if ($httpCode === 201 && $result['success']) {
    echo "Order created: " . $result['order_number'];
} else {
    echo "Error: " . $result['message'];
}
?>
```

---

## Important Notes

### Business Logic
- **Price Calculation:** Prices are calculated automatically from the database based on product and variation data
- **Order Number:** Generated automatically in format `ORD-YYYYMMDD-XXXX`
- **Status:** All new orders start with status `'new'`
- **Business & Location:** Hardcoded to business_id=1 and location_id=1

### Validation Rules
- **Customer name:** Required, maximum 255 characters
- **Customer phone:** Required, maximum 20 characters
- **Address, city, district:** Required fields
- **Items array:** Must contain at least 1 item
- **Product ID:** Must exist in the products table
- **Variation ID:** Must exist in variations table (if provided)
- **Quantity:** Must be integer, minimum value 1

### Error Handling
- Database transactions ensure data integrity
- Automatic rollback on any error
- Detailed validation error messages
- Exception logging for debugging

### Database Impact
- Creates record in `frontend_orders` table
- Creates records in `frontend_order_items` table
- Price calculations are real-time (not stored)

---

## Testing

### Test Data Requirements
Before testing, ensure your database has:
- At least one product with valid ID
- Product variations with prices set
- Products table with `sell_price_inc_tax` or `default_sell_price`

### Test Product IDs
Replace these with actual product IDs from your database:
```sql
-- Get available products for testing
SELECT id, name, sku FROM products LIMIT 5;

-- Get variations for a product
SELECT id, sub_sku, sell_price_inc_tax, default_sell_price 
FROM variations 
WHERE product_id = 1;
```

---

## Integration Checklist

- [ ] Set up POST route: `/api/orders`
- [ ] Configure CORS if frontend is on different domain
- [ ] Test with valid product IDs from your database
- [ ] Handle validation errors in frontend
- [ ] Implement loading states during API calls
- [ ] Add success/error notifications
- [ ] Test network error scenarios
- [ ] Verify price calculations are correct

**API is ready for production use!** 🚀
