# UltimatePOS Setup Guide: Mobile Phone Shop

## Business Overview
Setting up UltimatePOS for a mobile phone shop selling new phones, used phones, and accessories requires specific configuration to handle:

- **New Mobile Phones**: Brand new devices with warranty
- **Used Mobile Phones**: Refurbished/second-hand devices with condition tracking
- **Accessories**: Cases, screen protectors, chargers, headphones, etc.
- **IMEI Tracking**: Serial number management for warranty and theft prevention
- **Warranty Management**: Manufacturer and seller warranties
- **Condition Assessment**: For used phones (excellent, good, fair, poor)

## Step 1: Initial Business Setup

### 1.1 Business Registration
```php
// Business Details Configuration
$name = "Your Phone Shop Name";
$currency = "LKR"; // Sri Lankan Rupee
$timezone = "Asia/Colombo";
$fy_start_month = 1; // January
```

### 1.2 Business Settings
- **Tax Settings**: Configure GST/VAT rates for Sri Lanka
- **Invoice Settings**: Customize invoice layout for phone shop
- **Barcode Settings**: Configure for IMEI/product codes
- **Printer Settings**: Set up receipt printers

## Step 2: Product Catalog Structure

### 2.1 Create Product Categories
```
📱 Mobile Phones
├── 📱 New Phones
│   ├── Apple iPhone
│   ├── Samsung Galaxy
│   ├── Huawei
│   ├── Xiaomi
│   └── OnePlus
├── 🔄 Used Phones
│   ├── Apple iPhone (Used)
│   ├── Samsung Galaxy (Used)
│   ├── Huawei (Used)
│   └── Other Brands (Used)
└── 🔌 Accessories
    ├── Cases & Covers
    ├── Screen Protectors
    ├── Chargers & Cables
    ├── Headphones & Earphones
    ├── Power Banks
    └── Memory Cards
```

### 2.2 Product Types Configuration

#### New Mobile Phones
```php
// Product Configuration
[
    'type' => 'single', // or 'variable' for different storage/colors
    'enable_stock' => true,
    'alert_quantity' => 2,
    'sku' => 'IMEI_NUMBER', // Use IMEI as SKU
    'barcode_type' => 'C128',
    'tax_type' => 'inclusive',
    'warranty' => true,
    'warranty_period' => '12 months'
]
```

#### Used Mobile Phones
```php
// Product Configuration
[
    'type' => 'single',
    'enable_stock' => true,
    'alert_quantity' => 1,
    'sku' => 'IMEI_NUMBER',
    'barcode_type' => 'C128',
    'tax_type' => 'inclusive',
    'condition' => 'excellent|good|fair|poor',
    'warranty' => true,
    'warranty_period' => '3-6 months'
]
```

#### Accessories
```php
// Product Configuration
[
    'type' => 'single',
    'enable_stock' => true,
    'alert_quantity' => 5,
    'sku' => 'ACC_' . uniqid(),
    'barcode_type' => 'C128',
    'tax_type' => 'inclusive'
]
```

## Step 3: Custom Fields Setup

### 3.1 Product Custom Fields
Create custom fields for products:

#### For All Phones:
- **IMEI Number**: Text field (required)
- **Brand**: Dropdown (Apple, Samsung, Huawei, Xiaomi, OnePlus, etc.)
- **Model**: Text field
- **Color**: Dropdown (Black, White, Gold, Silver, Blue, etc.)
- **Storage**: Dropdown (64GB, 128GB, 256GB, 512GB, 1TB)
- **RAM**: Dropdown (4GB, 6GB, 8GB, 12GB, 16GB)

#### For New Phones:
- **Warranty Status**: Dropdown (Active, Expired)
- **Purchase Date**: Date field
- **Supplier Invoice**: File upload

#### For Used Phones:
- **Condition**: Dropdown (Excellent, Good, Fair, Poor)
- **Previous Owner**: Text field
- **Purchase Price**: Number field
- **Refurbished Date**: Date field
- **Testing Status**: Dropdown (Tested, Not Tested)

#### For Accessories:
- **Compatibility**: Text field (e.g., "iPhone 12-15", "Samsung Galaxy S21+")
- **Material**: Dropdown (Plastic, Metal, Silicone, Leather)
- **Warranty**: Dropdown (Yes, No)

### 3.2 Transaction Custom Fields
- **Customer Phone Number**: For warranty registration
- **Delivery Address**: For home delivery
- **Installation Required**: Checkbox
- **Data Transfer**: Checkbox (for phone sales)

## Step 4: IMEI Tracking System

### 4.1 IMEI as Product Identifier
```php
// Configure IMEI as primary identifier
$product_config = [
    'use_imei_as_sku' => true,
    'imei_validation' => true,
    'imei_format' => '/^[0-9]{15}$/', // 15-digit IMEI
    'duplicate_imei_check' => true
];
```

### 4.2 IMEI Management Features
- **IMEI Validation**: Check IMEI format and validity
- **Duplicate Prevention**: Prevent duplicate IMEI entries
- **IMEI Search**: Quick search by IMEI number
- **IMEI History**: Track IMEI through sales/purchase history

### 4.3 IMEI Integration
```php
// Custom IMEI validation function
function validateIMEI($imei) {
    // Remove any spaces or hyphens
    $imei = preg_replace('/[^0-9]/', '', $imei);

    // Check length (15 digits for IMEI)
    if (strlen($imei) !== 15) {
        return false;
    }

    // Luhn algorithm check
    $sum = 0;
    for ($i = 0; $i < 14; $i++) {
        $digit = (int)$imei[$i];
        if ($i % 2 === 1) {
            $digit *= 2;
            if ($digit > 9) {
                $digit -= 9;
            }
        }
        $sum += $digit;
    }

    $checkDigit = (10 - ($sum % 10)) % 10;
    return $checkDigit === (int)$imei[14];
}
```

## Step 5: Pricing Strategy

### 5.1 Price Groups Setup
Create different price groups for different customer segments:

#### Price Groups:
1. **Retail Price**: Standard retail pricing
2. **Wholesale Price**: For bulk buyers
3. **Dealer Price**: For phone dealers
4. **VIP Price**: For premium customers

### 5.2 Dynamic Pricing for Used Phones
```php
// Pricing based on condition
$conditionPricing = [
    'excellent' => 1.0,    // 100% of base price
    'good' => 0.8,         // 80% of base price
    'fair' => 0.6,         // 60% of base price
    'poor' => 0.4          // 40% of base price
];

// Age-based depreciation
$ageDepreciation = [
    '0-6 months' => 0.9,
    '6-12 months' => 0.8,
    '1-2 years' => 0.7,
    '2+ years' => 0.5
];
```

### 5.3 Profit Margin Tracking
- **New Phones**: 10-20% margin
- **Used Phones**: 20-40% margin
- **Accessories**: 30-50% margin

## Step 6: Supplier Management

### 6.1 Supplier Categories
```
📦 Suppliers
├── 🏭 Brand Authorized Dealers
│   ├── Apple Authorized Dealer
│   ├── Samsung Authorized Dealer
│   └── Huawei Authorized Dealer
├── 🏪 Wholesale Distributors
│   ├── Local Distributors
│   └── International Suppliers
└── 🔄 Used Phone Suppliers
    ├── Individual Sellers
    └── Bulk Used Phone Dealers
```

### 6.2 Supplier Configuration
```php
$supplier_config = [
    'supplier_type' => 'authorized_dealer|distributor|individual',
    'payment_terms' => 'cash|credit|installment',
    'warranty_support' => true,
    'return_policy' => '30_days|7_days|no_returns',
    'bulk_discount_eligible' => true
];
```

## Step 7: Customer Management

### 7.1 Customer Segmentation
```
👥 Customers
├── 🛒 Walk-in Customers
├── 📞 Regular Customers
├── 🏪 Bulk Buyers
└── 🔧 Service Customers
```

### 7.2 Customer Custom Fields
- **Phone Number**: Primary contact
- **Secondary Phone**: Alternative contact
- **Email**: For receipts and updates
- **Address**: For delivery
- **Preferred Brands**: Customer preferences
- **Purchase History**: Track buying patterns
- **Warranty Claims**: Service history

### 7.3 Loyalty Program
```php
$loyalty_config = [
    'points_per_purchase' => 1, // 1 point per LKR 100
    'points_value' => 1, // 1 LKR per point
    'minimum_points_for_redeem' => 100,
    'points_expiry' => '12_months'
];
```

## Step 8: Sales Process Configuration

### 8.1 POS Screen Customization
```php
$pos_config = [
    'show_imei_field' => true,
    'require_imei_scan' => true,
    'show_condition' => true,
    'warranty_registration' => true,
    'customer_phone_required' => true,
    'delivery_option' => true
];
```

### 8.2 Invoice Customization
- **Header**: Shop name, address, contact details
- **Product Details**: Include IMEI, condition, warranty info
- **Footer**: Terms & conditions, warranty information
- **QR Code**: For digital verification

### 8.3 Payment Methods
```php
$payment_methods = [
    'cash' => ['enabled' => true, 'default' => true],
    'card' => ['enabled' => true, 'gateway' => 'stripe'],
    'bank_transfer' => ['enabled' => true],
    'installment' => ['enabled' => true, 'max_months' => 24],
    'trade_in' => ['enabled' => true] // For used phone trade-ins
];
```

## Step 9: Inventory Management

### 9.1 Stock Alerts
```php
$stock_alerts = [
    'new_phones' => ['alert_quantity' => 2],
    'used_phones' => ['alert_quantity' => 1],
    'accessories' => ['alert_quantity' => 5],
    'low_stock_notification' => true,
    'auto_reorder' => true
];
```

### 9.2 Stock Tracking Features
- **IMEI-based tracking**: Track each device individually
- **Condition monitoring**: Regular condition assessments
- **Warranty tracking**: Monitor warranty expiry dates
- **Location tracking**: Track stock across multiple locations

## Step 10: Reporting & Analytics

### 10.1 Key Reports for Phone Shop
```php
$reports_config = [
    'sales_by_category' => true,
    'sales_by_brand' => true,
    'imei_tracking_report' => true,
    'warranty_expiry_report' => true,
    'condition_wise_report' => true,
    'profit_margin_analysis' => true,
    'customer_purchase_history' => true,
    'supplier_performance' => true
];
```

### 10.2 Dashboard Widgets
- **Top Selling Phones**: By brand and model
- **Low Stock Alerts**: Critical inventory warnings
- **Warranty Expiry**: Upcoming warranty expirations
- **Sales Trends**: Daily/weekly/monthly sales
- **Profit Margins**: By category and product

## Step 11: Warranty Management

### 11.1 Warranty Types
```php
$warranty_types = [
    'manufacturer_warranty' => [
        'duration' => '12_months',
        'covers' => 'hardware_defects',
        'exclusions' => 'physical_damage'
    ],
    'seller_warranty' => [
        'duration' => '6_months',
        'covers' => 'hardware_software',
        'conditions' => 'no_physical_damage'
    ],
    'extended_warranty' => [
        'duration' => '24_months',
        'cost' => 'additional_charge',
        'covers' => 'comprehensive'
    ]
];
```

### 11.2 Warranty Tracking
- **Warranty Registration**: Automatic during sale
- **Claim Processing**: Track warranty claims
- **Service History**: Maintain repair records
- **Expiry Alerts**: Notify before expiry

## Step 12: Integration Setup

### 12.1 Third-party Integrations
```php
$integrations = [
    'imei_checker' => [
        'enabled' => true,
        'api_key' => 'your_api_key',
        'check_blacklist' => true,
        'check_stolen' => true
    ],
    'sms_gateway' => [
        'enabled' => true,
        'provider' => 'twilio',
        'warranty_reminders' => true,
        'delivery_updates' => true
    ],
    'email_service' => [
        'enabled' => true,
        'receipts' => true,
        'warranty_info' => true
    ]
];
```

### 12.2 Mobile App Integration
- **Customer App**: For order tracking and warranty info
- **Staff App**: For inventory management and sales
- **Service App**: For warranty claim processing

## Step 13: Staff Management

### 13.1 User Roles for Phone Shop
```php
$user_roles = [
    'manager' => [
        'permissions' => ['all'],
        'access_level' => 'full'
    ],
    'sales_staff' => [
        'permissions' => ['sell', 'customers', 'reports'],
        'restrictions' => ['no_price_edit', 'no_delete']
    ],
    'technician' => [
        'permissions' => ['service', 'warranty', 'inventory'],
        'special_access' => ['imei_tracking']
    ],
    'inventory_staff' => [
        'permissions' => ['products', 'stock', 'suppliers'],
        'restrictions' => ['no_sales', 'no_financial']
    ]
];
```

### 13.2 Commission Structure
```php
$commission_config = [
    'sales_commission' => [
        'new_phones' => '2%',      // 2% on new phone sales
        'used_phones' => '3%',     // 3% on used phone sales
        'accessories' => '5%',     // 5% on accessory sales
        'monthly_target' => 'LKR_500,000'
    ],
    'technician_commission' => [
        'repair_service' => '10%', // 10% of repair cost
        'warranty_service' => '5%'  // 5% of warranty service
    ]
];
```

## Step 14: Backup & Security

### 14.1 Data Backup
```php
$backup_config = [
    'frequency' => 'daily',
    'retention' => '30_days',
    'include_attachments' => true,
    'cloud_backup' => true,
    'auto_backup' => true
];
```

### 14.2 Security Measures
- **IMEI Blacklist Check**: Prevent stolen phone sales
- **Customer Data Protection**: Secure customer information
- **Access Control**: Role-based permissions
- **Audit Trail**: Track all system changes

## Implementation Timeline

### Phase 1: Foundation (Week 1-2)
- Business setup and configuration
- Product categories and basic structure
- User roles and permissions

### Phase 2: Core Features (Week 3-4)
- Product catalog setup
- IMEI tracking system
- Basic sales process

### Phase 3: Advanced Features (Week 5-6)
- Warranty management
- Customer loyalty program
- Advanced reporting

### Phase 4: Integration & Testing (Week 7-8)
- Third-party integrations
- Staff training
- System testing

### Phase 5: Go-Live (Week 9)
- Final configuration
- Data migration (if applicable)
- Live operations

## Success Metrics

### Key Performance Indicators (KPIs)
- **Daily Sales**: Track daily revenue targets
- **Inventory Turnover**: Monitor stock movement
- **Customer Satisfaction**: Warranty claims and returns
- **Profit Margins**: By category and product type
- **Staff Performance**: Sales per staff member

### Monitoring Tools
- Real-time sales dashboard
- Inventory alerts system
- Customer feedback tracking
- Performance analytics

This comprehensive setup will transform UltimatePOS into a powerful mobile phone shop management system tailored to your specific business needs.
