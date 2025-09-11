# THENUKA MOBILE - Complete UltimatePOS Setup Guide
*Customized Setup for Used Phone Business with Warranty & Exchange Policy*

## Business Information
- **Business Name**: THENUKA MOBILE
- **Business Registration**: BL0001
- **Address**: No. 109, Udumahala Market, Anuradhapura
- **City**: Anuradhapura
- **Postal Code**: 50000
- **Province**: North Central Province
- **Country**: Sri Lanka

## Business Policies
- **Used Phone Warranty**: 6 months
- **Phone Exchange Policy**: 10 days phone-to-phone change
- **Business Type**: Mobile phone sales (New & Used)

## User Hierarchy
1. **Super Admin** - Full system control
2. **Admin** - Business management
3. **Cashier 1** - Sales operations
4. **Cashier 2** - Sales operations

---

## Table of Contents
1. [Initial System Setup](#initial-system-setup)
2. [Business Configuration](#business-configuration)
3. [User Management Setup](#user-management-setup)
4. [Tax Configuration for Sri Lanka](#tax-configuration-for-sri-lanka)
5. [Business Location Setup](#business-location-setup)
6. [Product Categories for Phone Business](#product-categories-for-phone-business)
7. [Warranty & Exchange Policy Setup](#warranty--exchange-policy-setup)
8. [Customer Management](#customer-management)
9. [Product Creation with Warranty](#product-creation-with-warranty)
10. [Sales Process with Exchange Policy](#sales-process-with-exchange-policy)
11. [Reporting & Analytics](#reporting--analytics)
12. [Daily Operations Guide](#daily-operations-guide)

---

## Initial System Setup

### Step 1: Business Information Configuration
```
Navigate to: Settings → Business Settings → Business Information

Business Details:
✓ Business Name: "THENUKA MOBILE"
✓ Start Date: [Your business start date]
✓ Financial Year Start: January
✓ Currency: LKR (Sri Lankan Rupee)
✓ Time Zone: Asia/Colombo
✓ Country: Sri Lanka
✓ State/Province: North Central Province
✓ City: Anuradhapura
✓ ZIP Code: 50000
✓ Business Address: "No. 109, Udumahala Market, Anuradhapura"
✓ Mobile Number: [Your primary contact]
✓ Alternate Number: [Secondary contact]
✓ Email: [Your business email]
✓ Website: [If you have one]
```

### Step 2: Business Registration Details
```
Tax Label 1: "VAT"
Tax Number 1: BL0001
Tax Label 2: "Business Registration"
Tax Number 2: BL0001

Default Settings:
✓ Default Profit Percent: 30% (adjust for used phones)
✓ Default Sales Discount: 0%
✓ Accounting Method: FIFO
✓ Sell Price Tax: Includes
```

### Step 3: Business Preferences for Phone Shop
```
Navigate to: Settings → Business Settings → Business Preferences

Essential Settings:
✓ Enable Tooltip: Yes
✓ Enable Stock: Yes
✓ Enable Product Expiry: Yes (for warranty tracking)
✓ Enable Lot Number: Yes (for IMEI tracking)
✓ Transaction Edit Days: 10 (matches your exchange policy)
✓ Enable Inline Tax: Yes
✓ Item Addition Method: Add item in new row
✓ Enable Brand: Yes
✓ Enable Category: Yes
✓ Enable Sub Category: Yes
```

---

## User Management Setup

### Step 1: Create User Roles

#### Super Admin Role (Already exists - modify if needed)
```
Navigate to: User Management → Roles → Edit Super Admin

Permissions:
✓ All permissions enabled
✓ Access to all modules
✓ Can view all contacts
✓ Can edit business settings
✓ Can manage users and roles
✓ Access to all reports
✓ Can delete transactions
```

#### Admin Role
```
Navigate to: User Management → Roles → Add Role

Role Name: "Admin"
Description: "Store manager with business oversight"

Permissions:
✓ View/Add/Edit/Delete products
✓ View/Add/Edit purchases
✓ View/Add/Edit sales
✓ View/Add/Edit contacts
✓ Access to all reports
✓ Can perform stock adjustments
✓ Can access POS
✓ Can view business settings (not edit)
✓ Can manage cash registers
✓ Access to expense management
✓ Can view all sales (not just own)
```

#### Cashier Role
```
Navigate to: User Management → Roles → Add Role

Role Name: "Cashier"
Description: "Sales staff with POS access"

Permissions:
✓ View/Add/Edit contacts (customers only)
✓ Add/Edit sales
✓ View own sales only
✓ Access to POS module
✓ Cannot access purchases
✓ Cannot view/edit business settings
✓ Cannot access reports (except basic sales)
✓ Cannot perform stock adjustments
✓ Can process returns/exchanges
```

### Step 2: Create User Accounts
```
Navigate to: User Management → Users → Add User

Super Admin User:
✓ Prefix: Mr./Ms.
✓ First Name: [Owner first name]
✓ Last Name: [Owner last name]
✓ Username: "superadmin"
✓ Email: [Owner email]
✓ Password: [Strong password]
✓ Role: Super Admin
✓ Allow Login: Yes

Admin User:
✓ Prefix: Mr./Ms.
✓ First Name: [Manager first name]
✓ Last Name: [Manager last name]
✓ Username: "admin"
✓ Email: [Manager email]
✓ Password: [Strong password]
✓ Role: Admin
✓ Allow Login: Yes

Cashier 1:
✓ Prefix: Mr./Ms.
✓ First Name: [Cashier 1 name]
✓ Last Name: [Cashier 1 surname]
✓ Username: "cashier1"
✓ Email: [Cashier 1 email]
✓ Password: [Strong password]
✓ Role: Cashier
✓ Sales Commission: 0% (or your commission rate)
✓ Allow Login: Yes

Cashier 2:
✓ Prefix: Mr./Ms.
✓ First Name: [Cashier 2 name]
✓ Last Name: [Cashier 2 surname]
✓ Username: "cashier2"
✓ Email: [Cashier 2 email]
✓ Password: [Strong password]
✓ Role: Cashier
✓ Sales Commission: 0% (or your commission rate)
✓ Allow Login: Yes
```

---

## Tax Configuration for Sri Lanka

### Step 1: Create Tax Rates
```
Navigate to: Settings → Tax Rates → Add Tax Rate

VAT (Value Added Tax):
✓ Name: "VAT 18%"
✓ Amount: 18.00
✓ Is Tax Group: No

NBT (Nation Building Tax) - if applicable:
✓ Name: "NBT 2%"
✓ Amount: 2.00
✓ Is Tax Group: No

Tax Exempt (for certain items):
✓ Name: "Tax Exempt"
✓ Amount: 0.00
✓ Is Tax Group: No
```

### Step 2: Set Default Tax
```
Navigate to: Settings → Business Settings → Tax

Default Sales Tax: VAT 18%
```

---

## Business Location Setup

### Step 1: Configure Main Store Location
```
Navigate to: Settings → Business Locations → Add Location

Location Details:
✓ Location Name: "THENUKA MOBILE - Main Store"
✓ Landmark: "Udumahala Market"
✓ Country: "Sri Lanka"
✓ State: "North Central Province"
✓ City: "Anuradhapura"
✓ ZIP Code: "50000"
✓ Address: "No. 109, Udumahala Market, Anuradhapura"
✓ Mobile: [Your store contact number]
✓ Alternate Number: [If available]
✓ Email: [Store email]
```

### Step 2: Invoice Scheme Setup
```
Navigate to: Settings → Invoice Settings → Invoice Schemes

Sales Invoice Scheme:
✓ Name: "Sales Invoice"
✓ Scheme Type: Year
✓ Prefix: "TM-INV"
✓ Start Number: 1
✓ Total Digits: 4
✓ Is Default: Yes

Purchase Invoice Scheme:
✓ Name: "Purchase Invoice"
✓ Scheme Type: Year
✓ Prefix: "TM-PUR"
✓ Start Number: 1
✓ Total Digits: 4
```

### Step 3: Invoice Layout with Warranty Information
```
Navigate to: Settings → Invoice Settings → Invoice Layouts

Create Invoice Layout:
✓ Name: "THENUKA MOBILE Invoice"
✓ Header Text: "Thank you for choosing THENUKA MOBILE!"
✓ Invoice Heading: "INVOICE"
✓ Sub Total Label: "Sub Total:"
✓ Discount Label: "Discount:"
✓ Tax Label: "VAT:"
✓ Total Label: "Total Amount:"

Business Information Display:
✓ Show Business Name: Yes
✓ Show Location Name: Yes
✓ Show City: Yes
✓ Show State: Yes
✓ Show ZIP Code: Yes
✓ Show Country: Yes
✓ Show Mobile: Yes
✓ Show Email: Yes
✓ Show Tax Number: Yes

Product Information Display:
✓ Show Brand: Yes
✓ Show SKU: Yes
✓ Show Serial Number: Yes (for IMEI)
✓ Show Lot Number: Yes (for IMEI tracking)

Footer Text: 
"WARRANTY POLICY: Used phones come with 6 months warranty. 
EXCHANGE POLICY: 10 days phone-to-phone exchange available.
Keep this receipt for warranty claims and exchanges.
Contact: [Your phone number] | Email: [Your email]"
```

---

## Product Categories for Phone Business

### Step 1: Create Main Categories
```
Navigate to: Products → Categories → Add Category

1. Mobile Phones
   - Short Code: MOB
   - Parent: None
   - Description: "All mobile phone categories"

2. Accessories
   - Short Code: ACC
   - Parent: None
   - Description: "Phone accessories and add-ons"
```

### Step 2: Create Sub-Categories
```
Sub-Categories under Mobile Phones:

1. Brand New Phones
   - Parent: Mobile Phones
   - Short Code: NEW
   - Description: "Brand new phones with manufacturer warranty"

2. Used Phones - 6 Month Warranty
   - Parent: Mobile Phones
   - Short Code: USED6M
   - Description: "Used phones with 6-month THENUKA MOBILE warranty"

3. Used Phones - As Is
   - Parent: Mobile Phones
   - Short Code: USEDASIS
   - Description: "Used phones sold as-is without warranty"

Sub-Categories under Accessories:

1. Cases & Covers
   - Parent: Accessories
   - Short Code: CASE

2. Screen Protectors
   - Parent: Accessories
   - Short Code: SCRN

3. Chargers & Cables
   - Parent: Accessories
   - Short Code: CHG

4. Headphones & Audio
   - Parent: Accessories
   - Short Code: AUDIO

5. Memory Cards
   - Parent: Accessories
   - Short Code: MEM
```

### Step 3: Add Phone Brands
```
Navigate to: Products → Brands → Add Brand

Major Brands in Sri Lankan Market:
✓ Apple - iPhone series
✓ Samsung - Galaxy series
✓ Huawei - P and Nova series
✓ Xiaomi - Redmi and Mi series
✓ Oppo - Popular in Sri Lanka
✓ Vivo - Popular mid-range phones
✓ OnePlus - Premium Android phones
✓ Nokia - Feature and smartphones
✓ Realme - Budget smartphones
✓ Sony - Xperia series
✓ LG - Various models
✓ Motorola - Moto series
✓ Honor - Huawei sub-brand
✓ Nothing - New brand
✓ Google - Pixel series
```

---

## Warranty & Exchange Policy Setup

### Step 1: Create Warranty Templates
```
Navigate to: Products → Warranties → Add Warranty

6-Month Used Phone Warranty:
✓ Warranty Name: "6 Month Used Phone Warranty"
✓ Description: "THENUKA MOBILE 6-month warranty for used phones"
✓ Duration: 6
✓ Duration Type: Months

Manufacturer Warranty (New Phones):
✓ Warranty Name: "Manufacturer Warranty"
✓ Description: "Original manufacturer warranty"
✓ Duration: 12
✓ Duration Type: Months

No Warranty (As-Is):
✓ Warranty Name: "No Warranty"
✓ Description: "Sold as-is without warranty"
✓ Duration: 0
✓ Duration Type: Days
```

### Step 2: Configure Exchange Policy Settings
```
Navigate to: Settings → Business Settings → Business Preferences

Transaction Edit Days: 10
(This allows modifications within your 10-day exchange period)

Return Policy Configuration:
✓ Enable product returns: Yes
✓ Return within days: 10
✓ Return conditions: Phone-to-phone exchange only
✓ Restocking fee: 0% (or as per your policy)
```

### Step 3: Create Exchange Policy Documentation
```
Create a document template for exchange policy:

THENUKA MOBILE EXCHANGE POLICY:
1. Exchange period: 10 days from purchase date
2. Exchange type: Phone-to-phone only (no cash refunds)
3. Condition: Phone must be in same condition as sold
4. Requirements: Original receipt and accessories
5. Eligibility: Manufacturing defects or customer dissatisfaction
6. Process: Customer selects replacement phone of equal or higher value
7. Price difference: Customer pays difference if upgrading
8. IMEI verification: Original phone IMEI must match receipt
```

---

## Customer Management

### Step 1: Create Customer Groups
```
Navigate to: Contacts → Customer Groups → Add Customer Group

1. Regular Customer
   - Name: "Regular Customer"
   - Discount: 0%
   - Price Calculation Type: Percentage

2. Wholesale Customer
   - Name: "Wholesale Customer"
   - Discount: 10%
   - Price Calculation Type: Percentage

3. VIP Customer
   - Name: "VIP Customer"
   - Discount: 5%
   - Price Calculation Type: Percentage

4. Exchange Customer
   - Name: "Exchange Customer"
   - Discount: 0%
   - Price Calculation Type: Percentage
   - Note: "For tracking exchange transactions"
```

### Step 2: Create Default Walk-in Customer
```
Navigate to: Contacts → Add Contact

Customer Details:
✓ Contact Type: Customer
✓ Name: "Walk-in Customer"
✓ Customer Group: Regular Customer
✓ Mobile: "0000000000"
✓ City: "Anuradhapura"
✓ State: "North Central Province"
✓ Country: "Sri Lanka"
✓ Is Default: Yes
```

---

## Product Creation with Warranty

### Step 1: Create Variation Templates
```
Navigate to: Products → Variation Templates

Storage Capacity Template:
✓ Template Name: "Storage"
✓ Values: 32GB, 64GB, 128GB, 256GB, 512GB, 1TB

Color Template:
✓ Template Name: "Color"
✓ Values: Black, White, Blue, Red, Pink, Gold, Silver, Green, Purple, Rose Gold

Condition Template (for used phones):
✓ Template Name: "Condition"
✓ Values: Excellent (95-100%), Good (85-94%), Fair (70-84%)

Network Template:
✓ Template Name: "Network"
✓ Values: Single SIM, Dual SIM, eSIM
```

### Step 2: Create Used Phone Product Example
```
Navigate to: Products → Add Product

Product Details:
✓ Product Name: "iPhone 13 (Used)"
✓ Product Type: Variable
✓ Category: Mobile Phones → Used Phones - 6 Month Warranty
✓ Brand: Apple
✓ Unit: Piece
✓ SKU: Auto-generated
✓ Barcode Type: EAN-13

Tax Settings:
✓ Tax Rate: VAT 18%
✓ Tax Type: Inclusive

Stock Settings:
✓ Enable Stock: Yes
✓ Alert Quantity: 2

Warranty Settings:
✓ Warranty: 6 Month Used Phone Warranty

Product Description:
"Used iPhone 13 in excellent condition. Tested and verified. 
Comes with 6-month THENUKA MOBILE warranty.
10-day phone-to-phone exchange available.
IMEI: [Individual IMEI for each unit]
Condition: [Specific condition assessment]"

Custom Fields:
✓ Custom Field 1: "IMEI Number"
✓ Custom Field 2: "Battery Health %"
✓ Custom Field 3: "Original Box Included"
✓ Custom Field 4: "Accessories Included"
```

### Step 3: Apply Variations and Set Pricing
```
Apply Templates:
✓ Storage Capacity
✓ Color
✓ Condition

Generated Variations (example):
- iPhone 13 (Used) - 128GB - Black - Excellent
- iPhone 13 (Used) - 128GB - Black - Good
- iPhone 13 (Used) - 128GB - Blue - Excellent
- iPhone 13 (Used) - 256GB - Black - Excellent

Pricing Strategy:
Excellent Condition (95-100%):
✓ 128GB: LKR 180,000 → LKR 220,000
✓ 256GB: LKR 210,000 → LKR 250,000

Good Condition (85-94%):
✓ 128GB: LKR 160,000 → LKR 200,000
✓ 256GB: LKR 190,000 → LKR 230,000

Fair Condition (70-84%):
✓ 128GB: LKR 140,000 → LKR 180,000
✓ 256GB: LKR 170,000 → LKR 210,000
```

---

## Sales Process with Exchange Policy

### Step 1: Regular Sale Process
```
1. Customer Selection:
   - Search existing customer or add new
   - Select customer group for appropriate pricing

2. Product Selection:
   - Search for product (e.g., "iPhone 13 Used")
   - Select specific variation (storage, color, condition)
   - Choose specific IMEI from lot numbers
   - Verify phone condition and accessories

3. Sale Details:
   - Add product to cart
   - Apply discount if applicable
   - Add accessories if customer wants
   - Calculate total with tax

4. Warranty Documentation:
   - Add warranty information to invoice
   - Record warranty start date (sale date)
   - Record warranty end date (sale date + 6 months)
   - Include warranty terms on receipt

5. Payment & Completion:
   - Process payment (cash/card)
   - Print invoice with warranty info
   - Provide customer with warranty terms
   - Record IMEI for tracking
```

### Step 2: Exchange Transaction Process
```
When Customer Returns for Exchange (within 10 days):

Step 1: Verify Eligibility
✓ Check original invoice (within 10 days)
✓ Verify IMEI matches original sale
✓ Inspect phone condition
✓ Confirm accessories are complete
✓ Check reason for exchange

Step 2: Process Return
1. Go to: Sales → List Sales
2. Find original transaction
3. Click "Edit" or "Add Return"
4. Create return entry:
   - Return Type: Exchange
   - Return Reason: [Customer reason]
   - Product: Original phone
   - Quantity: 1
   - Return Amount: Original selling price

Step 3: Process New Sale
1. Create new sale transaction
2. Customer: Same customer
3. Product: New phone selected by customer
4. Calculate price difference:
   - If new phone costs more: Customer pays difference
   - If new phone costs less: Issue store credit
5. Complete new sale with warranty

Step 4: Update Records
✓ Mark original IMEI as returned
✓ Add new IMEI to customer record
✓ Update inventory for both phones
✓ Issue new warranty for replacement phone
```

### Step 3: Exchange Policy Enforcement
```
System Configurations:

1. Transaction Edit Period:
   Settings → Business Settings → Business Preferences
   ✓ Transaction Edit Days: 10

2. Return Permissions:
   User Management → Roles → Edit Cashier Role
   ✓ Can process returns: Yes
   ✓ Return approval required: No (for simple exchanges)

3. Exchange Documentation:
   - Use transaction notes to record exchange details
   - Maintain exchange log in customer notes
   - Track exchange patterns for business analysis
```

---

## Payment Methods & Accounts

### Step 1: Create Payment Accounts
```
Navigate to: Settings → Payment Accounts → Add Account

Cash Register:
✓ Account Name: "THENUKA MOBILE Cash Register"
✓ Account Number: "TM-CASH-001"
✓ Account Type: Current Account
✓ Note: "Main cash register for daily sales"

Bank Account:
✓ Account Name: "THENUKA MOBILE Bank Account"
✓ Account Number: [Your actual bank account number]
✓ Account Type: Current Account
✓ Note: "Primary business banking account"

Card Payment Account:
✓ Account Name: "Card Payments"
✓ Account Number: "TM-CARD-001"
✓ Account Type: Current Account
✓ Note: "Credit/Debit card payments"
```

### Step 2: Configure Payment Methods
```
Navigate to: Settings → Business Settings → Payment Methods

Enable Payment Methods:
✓ Cash: Yes → Link to Cash Register account
✓ Card: Yes → Link to Card Payment account
✓ Bank Transfer: Yes → Link to Bank Account
✓ Other: Yes (for mobile payments, etc.)

Default Payment Method: Cash
```

---

## Inventory Management for IMEI Tracking

### Step 1: Stock Addition with IMEI Tracking
```
When Adding New Used Phones:

1. Go to: Purchases → Add Purchase
2. Select supplier (individual seller or wholesale)
3. Add product with specific variation
4. In "Lot Number" field, add:
   Format: IMEI|Condition|BatteryHealth|Accessories
   
   Example entries:
   123456789012345|Excellent|95%|Box+Charger
   123456789012346|Good|88%|Phone Only
   123456789012347|Excellent|92%|Box+Charger+Earphones

5. Each lot number represents one specific phone
6. Cost tracking per individual phone
```

### Step 2: Stock Management Process
```
Daily Stock Operations:

1. Receiving Used Phones:
   - Test all functions (calling, WiFi, camera, etc.)
   - Check battery health
   - Verify IMEI not blacklisted
   - Assess physical condition
   - Record all details in lot tracking
   - Add to inventory with specific condition grade

2. Stock Alerts:
   - Set alert quantity to 2 for popular models
   - Monitor fast-moving models
   - Track seasonal demand patterns

3. IMEI Verification:
   - Check IMEI against stolen phone databases
   - Verify original owner documentation
   - Confirm factory reset completed
   - Ensure iCloud/Google account removed
```

---

## Reporting & Analytics

### Step 1: Essential Reports for Phone Business
```
Daily Reports:
1. Sales Summary by Category
   - New phones vs Used phones
   - Sales by brand
   - Popular storage capacities
   - Color preferences

2. Warranty Tracking Report
   - Active warranties by month
   - Warranty claims received
   - Warranty expiry alerts

3. Exchange Report
   - Exchange requests by reason
   - Exchange patterns by product
   - Exchange rate percentage

4. IMEI Tracking Report
   - Phones sold by IMEI
   - Warranty status by IMEI
   - Available stock by condition
```

### Step 2: Monthly Analysis
```
Monthly Business Review:

1. Inventory Analysis:
   - Fast-moving vs slow-moving models
   - Condition grade performance
   - Profit margins by category
   - Age of inventory

2. Customer Analysis:
   - New vs repeat customers
   - Customer satisfaction (exchange rates)
   - Popular brands by customer segment

3. Financial Analysis:
   - Revenue by category
   - Profit margins by phone condition
   - Exchange cost impact
   - Warranty claim costs
```

---

## Daily Operations Guide

### Step 1: Daily Opening Procedures
```
Morning Checklist for Cashiers:

1. System Login:
   □ Login with individual credentials
   □ Check system date and time
   □ Verify cash register starting amount

2. Inventory Check:
   □ Review low stock alerts
   □ Check phones requiring warranty attention
   □ Verify high-value phones are secure
   □ Check IMEI blacklist updates (if available)

3. Customer Service Preparation:
   □ Review exchange requests from previous day
   □ Check warranty expiry notifications
   □ Prepare customer files for scheduled pickups
```

### Step 2: Sales Transaction Process
```
For Each Sale:

1. Customer Information:
   □ Search existing customer or create new
   □ Verify contact information
   □ Note any previous purchases/exchanges

2. Product Selection:
   □ Help customer choose appropriate phone
   □ Explain condition grading system
   □ Demonstrate phone functionality
   □ Show included accessories

3. Transaction Processing:
   □ Add product with correct variation
   □ Select specific IMEI from available stock
   □ Apply customer group discounts
   □ Calculate total with tax

4. Payment & Documentation:
   □ Process payment via chosen method
   □ Print invoice with warranty information
   □ Explain warranty terms and exchange policy
   □ Provide receipt and warranty documentation
   □ Record sale in IMEI tracking log
```

### Step 3: Exchange Process
```
For Exchange Requests:

1. Initial Verification:
   □ Check original receipt (within 10 days)
   □ Verify customer identity
   □ Inspect returned phone condition
   □ Confirm IMEI matches original sale
   □ Check accessories are complete

2. Exchange Approval:
   □ Determine exchange eligibility
   □ Explain available replacement options
   □ Calculate price differences
   □ Get customer agreement on new phone

3. Process Exchange:
   □ Create return transaction for original phone
   □ Create new sale for replacement phone
   □ Process payment difference (if any)
   □ Update IMEI tracking records
   □ Issue new warranty documentation

4. Documentation:
   □ Note exchange reason in customer record
   □ Update inventory for both phones
   □ File exchange documentation
   □ Inform manager of exchange (if required)
```

### Step 4: Daily Closing Procedures
```
End of Day Checklist:

1. Sales Reconciliation:
   □ Count cash register
   □ Verify card payment receipts
   □ Check sales total matches register
   □ Generate daily sales report

2. Inventory Security:
   □ Secure high-value phones
   □ Update IMEI tracking log
   □ Check warranty expiry alerts
   □ Prepare tomorrow's reorder list

3. System Maintenance:
   □ Backup important data (if required)
   □ Log out of all systems
   □ Secure physical documents
   □ Report any system issues to admin
```

---

## Staff Training Guide

### Step 1: Cashier Training Program
```
Week 1: Basic System Operations
□ User login and navigation
□ Customer search and creation
□ Product search and selection
□ Basic sales transaction processing
□ Payment method handling
□ Invoice printing

Week 2: Phone-Specific Operations
□ IMEI verification and tracking
□ Condition assessment basics
□ Warranty explanation to customers
□ Exchange policy procedures
□ Accessory upselling techniques
□ Customer service standards

Week 3: Advanced Operations
□ Return and exchange processing
□ Inventory checking procedures
□ Report generation basics
□ Problem escalation procedures
□ Daily opening and closing tasks
□ Emergency procedures
```

### Step 2: Key Knowledge Areas
```
All Staff Must Know:

1. Product Knowledge:
   □ Popular phone models and specifications
   □ Condition grading system (Excellent, Good, Fair)
   □ Battery health assessment
   □ Common phone problems and solutions
   □ Accessory compatibility

2. Policy Knowledge:
   □ 6-month warranty terms and conditions
   □ 10-day exchange policy details
   □ Return requirements and procedures
   □ Customer service standards
   □ Escalation procedures

3. System Knowledge:
   □ IMEI tracking importance
   □ Variation selection process
   □ Payment processing
   □ Basic troubleshooting
   □ Report generation
```

---

## Troubleshooting Guide

### Common Issues and Solutions

#### Issue 1: IMEI Not Found in System
```
Problem: Cannot locate specific IMEI during sale
Solution:
1. Check lot number format is correct
2. Verify IMEI was entered during purchase
3. Search by partial IMEI if needed
4. Check if phone was already sold
5. Contact admin if IMEI missing from system
```

#### Issue 2: Warranty Date Calculation
```
Problem: Warranty end date not calculating correctly
Solution:
1. Verify warranty template duration is set correctly
2. Check sale date is accurate
3. Ensure warranty is linked to product
4. Manually calculate if needed: Sale Date + 6 months
5. Update customer record with correct warranty info
```

#### Issue 3: Exchange Policy Conflicts
```
Problem: System won't allow exchange after certain period
Solution:
1. Check transaction edit days setting (should be 10)
2. Verify original sale date is within 10 days
3. Use manual override if necessary (admin approval)
4. Create new sale and return separately if needed
5. Document exception and inform management
```

#### Issue 4: Stock Levels Not Updating
```
Problem: Stock not reflecting correctly after sales
Solution:
1. Check if "Enable Stock" is turned on for product
2. Verify purchase transaction was completed properly
3. Check if lot number was used correctly
4. Perform stock adjustment if needed
5. Contact admin for system reset if required
```

---

## Contact Information for Support

### Internal Support
- **Super Admin**: [Contact details]
- **Admin**: [Contact details]
- **System Issues**: [IT support contact]

### External Support
- **UltimatePOS Support**: [Official support channels]
- **Technical Issues**: [System provider contact]

---

This complete setup guide is specifically customized for THENUKA MOBILE's business requirements, including the 6-month warranty for used phones, 10-day exchange policy, and specific user hierarchy. Follow each section systematically to set up your system correctly for optimal business operations.
