# StockManagementControl - User Flows

## Overview

User flows document the step-by-step journeys users take through StockManagementControl to accomplish their goals. These flows help ensure the application is intuitive, efficient, and meets user needs. Each flow is mapped to specific personas and includes decision points, error handling, and success criteria.

---

## Flow 1: User Authentication & Onboarding

### Persona: All Users (First-Time Access)
### Goal: Log in to the system and access the dashboard

### Flow Steps:

1. **Landing Page**
   - User navigates to application URL
   - System displays login page
   - User sees login form (email/username, password)
   - Optional: "Forgot Password?" link

2. **Login Attempt**
   - User enters credentials
   - User clicks "Login" button
   - System validates credentials
   - **Decision Point**: Valid credentials?

3. **Success Path**
   - System authenticates user
   - System generates JWT token
   - System stores token in localStorage
   - System redirects to Dashboard
   - Dashboard loads user-specific data

4. **Error Path**
   - System displays error message: "Invalid credentials"
   - User can retry login
   - Optional: "Forgot Password?" flow

5. **First-Time User (Registration)**
   - User clicks "Register" link
   - System displays registration form
   - User enters: name, email, password, role
   - System validates input
   - System creates account
   - System redirects to login

### Technical Implementation:
- **Route**: `/login` → `/dashboard`
- **Components**: `LoginView.vue`, `RegisterView.vue`
- **Store**: `authStore.login()`, `authStore.register()`
- **API**: `POST /api/auth/login`, `POST /api/auth/register`
- **Guard**: Route guard checks authentication token

### Success Criteria:
- User successfully logs in
- Token is stored securely
- User is redirected to dashboard
- User sees personalized dashboard content

---

## Flow 2: Viewing Low Stock Alerts

### Persona: Sarah (Store Manager), Michael (Warehouse Supervisor)
### Goal: Quickly identify products that need restocking

### Flow Steps:

1. **Dashboard Access**
   - User logs in (see Flow 1)
   - System loads Dashboard
   - Dashboard displays summary cards

2. **Low Stock Section**
   - Dashboard shows "Low Stock Alerts" card
   - Card displays count: "5 products need attention"
   - User clicks "View Alerts" button or card

3. **Low Stock List**
   - System navigates to Products view with filter applied
   - System displays filtered list of low stock products
   - Each product shows:
     - Product name
     - Current stock level
     - Minimum threshold
     - Days until stockout (if applicable)

4. **Product Details (Optional)**
   - User clicks on a product
   - System shows product details modal/page
   - User can see:
     - Full product information
     - Recent transactions
     - Supplier information
     - Order history

5. **Action Options**
   - User can:
     - Create purchase order (see Flow 4)
     - Adjust stock level (see Flow 5)
     - Dismiss alert (if false positive)
     - View supplier details

### Technical Implementation:
- **Route**: `/dashboard` → `/products?filter=low-stock`
- **Components**: `DashboardView.vue`, `ProductsView.vue`, `ProductCard.vue`
- **Store**: `productStore.fetchLowStockProducts()`
- **API**: `GET /api/products/low-stock`
- **Computed**: `filteredLowStockProducts` in store

### Success Criteria:
- User sees all products below threshold
- Information is accurate and up-to-date
- User can take action from the list
- Alerts update in real-time

---

## Flow 3: Adding a New Product

### Persona: Sarah (Store Manager), Michael (Warehouse Supervisor)
### Goal: Add a new product to the inventory system

### Flow Steps:

1. **Navigate to Products**
   - User is on Dashboard or any page
   - User clicks "Products" in navigation menu
   - System navigates to Products view

2. **Initiate Add Product**
   - User clicks "Add Product" button
   - System displays product form modal/page
   - Form shows required fields:
     - Product name *
     - SKU *
     - Category
     - Description
     - Initial stock quantity *
     - Unit price *
     - Cost price
     - Supplier
     - Minimum stock threshold *
     - Location (if multi-location)

3. **Fill Form**
   - User enters product information
   - System validates input in real-time:
     - Required fields highlighted if empty
     - SKU uniqueness checked
     - Price format validated
     - Stock quantity must be >= 0

4. **Submit Form**
   - User clicks "Save Product" button
   - System shows loading indicator
   - System sends POST request to API
   - **Decision Point**: Validation successful?

5. **Success Path**
   - API returns success response
   - System closes form modal
   - System adds product to list
   - System shows success message: "Product added successfully"
   - Product appears in products list

6. **Error Path**
   - API returns validation error
   - System displays error message
   - System highlights invalid fields
   - User can correct and resubmit

### Technical Implementation:
- **Route**: `/products` → `/products/new` (or modal)
- **Components**: `ProductsView.vue`, `ProductForm.vue`
- **Store**: `productStore.addProduct(product)`
- **API**: `POST /api/products`
- **Validation**: Frontend (Vue) + Backend (C#)

### Success Criteria:
- Product is saved to database
- Product appears in products list
- User receives confirmation
- Form resets for next entry

---

## Flow 4: Creating a Purchase Order

### Persona: Sarah (Store Manager), Michael (Warehouse Supervisor)
### Goal: Order products from a supplier to restock inventory

### Flow Steps:

1. **Identify Need**
   - User identifies low stock product (see Flow 2)
   - OR user navigates to Suppliers view
   - User selects a product or supplier

2. **Initiate Purchase Order**
   - User clicks "Create Purchase Order" button
   - System displays purchase order form
   - Form shows:
     - Supplier selector (dropdown)
     - Product selector (with search)
     - Quantity input
     - Unit price (pre-filled from supplier or product)
     - Expected delivery date
     - Notes

3. **Add Products to Order**
   - User selects supplier
   - System loads supplier's products
   - User adds products to order:
     - Select product
     - Enter quantity
     - Confirm price
     - Click "Add to Order"
   - Order summary updates in real-time

4. **Review Order**
   - User reviews order summary:
     - Total items
     - Total cost
     - Expected delivery date
   - User can:
     - Remove items
     - Adjust quantities
     - Update delivery date

5. **Submit Order**
   - User clicks "Submit Purchase Order"
   - System validates order:
     - At least one product
     - Valid supplier
     - Valid quantities
   - **Decision Point**: Validation successful?

6. **Success Path**
   - System creates purchase order
   - System sends notification to supplier (if integrated)
   - System shows success message
   - System redirects to Purchase Orders list
   - Order appears with status "Pending"

7. **Receive Order (Later Flow)**
   - When shipment arrives, user navigates to Purchase Orders
   - User clicks "Receive Order"
   - System updates stock levels
   - System marks order as "Received"

### Technical Implementation:
- **Route**: `/suppliers` → `/purchase-orders/new`
- **Components**: `PurchaseOrderForm.vue`, `SupplierSelector.vue`
- **Store**: `supplierStore.createPurchaseOrder()`, `productStore.updateStock()`
- **API**: `POST /api/purchase-orders`, `PUT /api/purchase-orders/{id}/receive`

### Success Criteria:
- Purchase order is created
- Order is tracked in system
- Stock levels update when received
- User can track order status

---

## Flow 5: Recording a Sale Transaction

### Persona: Sarah (Store Manager), Emma (Inventory Clerk)
### Goal: Record a customer sale and update inventory

### Flow Steps:

1. **Navigate to Sales**
   - User clicks "Sales" or "Transactions" in menu
   - System navigates to Transactions view
   - User selects "New Sale" option

2. **Select Customer**
   - System displays sale form
   - User can:
     - Select existing customer from dropdown
     - OR click "New Customer" to add customer
     - OR select "Walk-in Customer" (no customer required)

3. **Add Products to Sale**
   - User searches/selects products
   - For each product:
     - User enters quantity
     - System calculates line total (quantity × price)
     - System checks stock availability
     - **Decision Point**: Sufficient stock?

4. **Stock Check**
   - **Sufficient Stock**: Product added to cart
   - **Insufficient Stock**: System shows warning
     - User can:
       - Reduce quantity
       - Remove product
       - Proceed with available quantity

5. **Review Sale**
   - System displays sale summary:
     - Products and quantities
     - Subtotal
     - Tax (if applicable)
     - Total
   - User can:
     - Adjust quantities
     - Remove products
     - Apply discounts (if authorized)

6. **Complete Sale**
   - User clicks "Complete Sale"
   - System processes transaction:
     - Creates sale record
     - Updates stock levels
     - Records customer purchase (if customer selected)
     - Generates receipt/invoice
   - **Decision Point**: Transaction successful?

7. **Success Path**
   - System shows success message
   - System displays receipt
   - System updates dashboard (sales metrics)
   - User can:
     - Print receipt
     - Email receipt (if customer email provided)
     - Start new sale

8. **Error Path**
   - System displays error message
   - Transaction is not processed
   - User can retry or cancel

### Technical Implementation:
- **Route**: `/transactions` → `/transactions/sales/new`
- **Components**: `SaleForm.vue`, `ProductSelector.vue`, `CustomerSelector.vue`
- **Store**: `transactionStore.createSale()`, `productStore.updateStock()`
- **API**: `POST /api/transactions/sales`, `PUT /api/products/{id}/stock`

### Success Criteria:
- Sale is recorded
- Stock levels update correctly
- Receipt is generated
- Customer history is updated

---

## Flow 6: Generating Inventory Report

### Persona: David (Business Owner), Lisa (Accountant), Sarah (Store Manager)
### Goal: Generate a comprehensive inventory report for analysis

### Flow Steps:

1. **Navigate to Reports**
   - User clicks "Reports" in navigation menu
   - System navigates to Reports view
   - System displays report categories:
     - Inventory Reports
     - Sales Reports
     - Supplier Reports
     - Custom Reports

2. **Select Report Type**
   - User clicks "Inventory Reports"
   - System shows inventory report options:
     - Current Stock Levels
     - Inventory Valuation
     - Stock Movement History
     - Low Stock Report
     - Product Performance

3. **Configure Report**
   - User selects report type (e.g., "Inventory Valuation")
   - System displays filter options:
     - Date range
     - Category filter
     - Location filter (if multi-location)
     - Include/exclude zero stock items
   - User applies filters

4. **Generate Report**
   - User clicks "Generate Report"
   - System shows loading indicator
   - System fetches data from API
   - System processes and formats data

5. **View Report**
   - System displays report:
     - Summary statistics (total value, item count)
     - Detailed table/list
     - Visual charts (if applicable)
   - User can:
     - Scroll through data
     - Sort columns
     - Filter further

6. **Export Report**
   - User clicks "Export" button
   - System shows export options:
     - PDF
     - Excel (CSV)
     - Print
   - User selects format
   - System generates file
   - System downloads file to user's device

### Technical Implementation:
- **Route**: `/reports` → `/reports/inventory`
- **Components**: `ReportsView.vue`, `ReportGenerator.vue`, `ReportChart.vue`
- **Store**: `reportStore.generateReport()`, `reportStore.exportReport()`
- **API**: `GET /api/reports/inventory`, `POST /api/reports/export`

### Success Criteria:
- Report is generated accurately
- Data is current and correct
- Export works correctly
- Report is formatted professionally

---

## Flow 7: Adjusting Stock Levels (Stock Take)

### Persona: Michael (Warehouse Supervisor), Sarah (Store Manager)
### Goal: Correct stock levels after physical inventory count

### Flow Steps:

1. **Navigate to Products**
   - User navigates to Products view
   - User selects a product to adjust

2. **Initiate Adjustment**
   - User clicks "Adjust Stock" button
   - System displays stock adjustment form
   - Form shows:
     - Current stock level
     - Adjustment type selector:
       - Increase (add stock)
       - Decrease (remove stock)
       - Set to (set exact amount)
     - Quantity input
     - Reason selector:
       - Stock take correction
       - Damage/Loss
       - Found stock
       - Other (with notes)

3. **Enter Adjustment**
   - User selects adjustment type
   - User enters quantity
   - User selects reason
   - User adds notes (optional)
   - System calculates new stock level:
     - Increase: current + quantity
     - Decrease: current - quantity
     - Set to: quantity

4. **Review Adjustment**
   - System displays:
     - Current stock: X
     - Adjustment: ±Y
     - New stock: Z
   - User reviews and confirms

5. **Submit Adjustment**
   - User clicks "Apply Adjustment"
   - System validates:
     - New stock >= 0
     - Reason provided
   - **Decision Point**: Validation successful?

6. **Success Path**
   - System creates adjustment transaction
   - System updates product stock level
   - System records transaction in audit log
   - System shows success message
   - Product card updates with new stock level

7. **Low Stock Alert Check**
   - If new stock < minimum threshold:
     - System triggers low stock alert
     - Alert appears on dashboard
   - If stock was low and now above threshold:
     - System removes low stock alert

### Technical Implementation:
- **Route**: `/products/{id}` → `/products/{id}/adjust`
- **Components**: `ProductDetailView.vue`, `StockAdjustmentForm.vue`
- **Store**: `productStore.adjustStock()`, `transactionStore.createAdjustment()`
- **API**: `POST /api/products/{id}/adjust-stock`, `POST /api/transactions/adjustments`

### Success Criteria:
- Stock level is updated correctly
- Transaction is recorded
- Audit trail is maintained
- Alerts update appropriately

---

## Flow 8: Managing User Accounts (Admin)

### Persona: James (IT Administrator)
### Goal: Add new users and manage permissions

### Flow Steps:

1. **Access Admin Panel**
   - Admin user logs in
   - Admin clicks "Admin" or "Settings" in menu
   - System navigates to Admin/Users view
   - System displays list of all users

2. **Add New User**
   - Admin clicks "Add User" button
   - System displays user creation form:
     - Name *
     - Email *
     - Password * (or generate)
     - Role selector:
       - Admin
       - Manager
       - Staff
       - Viewer
     - Location access (if multi-location)
   - Admin fills form

3. **Assign Permissions**
   - Admin selects role
   - System shows permission summary:
     - Can view products: Yes/No
     - Can edit products: Yes/No
     - Can delete products: Yes/No
     - Can view reports: Yes/No
     - Can manage users: Yes/No
   - Admin reviews permissions

4. **Create User**
   - Admin clicks "Create User"
   - System validates:
     - Email format
     - Email uniqueness
     - Password strength
   - **Decision Point**: Validation successful?

5. **Success Path**
   - System creates user account
   - System sends welcome email (if configured)
   - System adds user to list
   - System shows success message
   - User can now log in

6. **Edit User**
   - Admin clicks "Edit" on existing user
   - System displays edit form (pre-filled)
   - Admin can:
     - Change role
     - Update permissions
     - Reset password
     - Deactivate account
   - Admin saves changes

7. **Deactivate User**
   - Admin clicks "Deactivate" on user
   - System shows confirmation dialog
   - Admin confirms
   - System deactivates user
   - User cannot log in
   - User's data is preserved

### Technical Implementation:
- **Route**: `/admin/users` → `/admin/users/new`
- **Components**: `UserManagementView.vue`, `UserForm.vue`
- **Store**: `authStore.createUser()`, `authStore.updateUser()`
- **API**: `POST /api/admin/users`, `PUT /api/admin/users/{id}`, `DELETE /api/admin/users/{id}`
- **Guard**: Requires admin role

### Success Criteria:
- User account is created
- Permissions are correctly assigned
- User can log in with new account
- Changes are reflected immediately

---

## Flow 9: Viewing Dashboard Analytics

### Persona: David (Business Owner), Sarah (Store Manager)
### Goal: Get quick overview of business performance

### Flow Steps:

1. **Access Dashboard**
   - User logs in (see Flow 1)
   - System automatically navigates to Dashboard
   - System loads dashboard data

2. **Dashboard Loads**
   - System displays multiple sections:
     - **Summary Cards**:
       - Total Products
       - Low Stock Alerts
       - Today's Sales
       - Total Inventory Value
     - **Charts**:
       - Sales Trend (last 7/30 days)
       - Top Selling Products
       - Stock Levels by Category
     - **Recent Activity**:
       - Latest transactions
       - Recent stock movements

3. **Interact with Dashboard**
   - User can:
     - Click cards to navigate to detailed views
     - Hover over charts for details
     - Click "View All" for expanded views
     - Change date range for charts

4. **Drill Down**
   - User clicks on "Low Stock Alerts" card
   - System navigates to low stock products (see Flow 2)
   - OR user clicks on sales chart
   - System navigates to sales report

5. **Refresh Data**
   - User clicks refresh button
   - System reloads dashboard data
   - System shows loading indicator
   - Dashboard updates with latest data

### Technical Implementation:
- **Route**: `/dashboard` (default route)
- **Components**: `DashboardView.vue`, `SummaryCard.vue`, `ChartComponent.vue`
- **Store**: `productStore.fetchSummary()`, `transactionStore.fetchRecent()`, `reportStore.fetchDashboardData()`
- **API**: `GET /api/dashboard/summary`, `GET /api/dashboard/charts`

### Success Criteria:
- Dashboard loads quickly
- Data is accurate and current
- Charts render correctly
- Navigation works smoothly

---

## Flow 10: Error Handling & Recovery

### Persona: All Users
### Goal: Handle errors gracefully and recover from failures

### Flow Steps:

1. **Error Occurs**
   - User performs an action (any flow)
   - System encounters error:
     - Network failure
     - Validation error
     - Server error
     - Permission denied

2. **Error Detection**
   - System catches error
   - System identifies error type
   - System determines user-friendly message

3. **Error Display**
   - System shows error notification:
     - **Network Error**: "Connection lost. Please check your internet."
     - **Validation Error**: "Please check the highlighted fields."
     - **Server Error**: "Something went wrong. Please try again."
     - **Permission Error**: "You don't have permission to perform this action."

4. **Recovery Options**
   - User can:
     - **Retry**: Click "Try Again" button
     - **Cancel**: Click "Cancel" to abort
     - **Go Back**: Navigate to previous page
     - **Contact Support**: Click "Report Issue" (for persistent errors)

5. **Auto-Recovery (Network)**
   - If network error:
     - System queues failed request
     - System shows "Retrying..." indicator
     - When connection restored:
       - System automatically retries
       - System shows success message

6. **Form Recovery**
   - If form submission fails:
     - System preserves form data
     - User can correct errors
     - User can resubmit without re-entering all data

### Technical Implementation:
- **Components**: `ErrorBoundary.vue`, `ErrorNotification.vue`
- **Store**: `errorStore.handleError()`, `errorStore.clearError()`
- **Middleware**: Error handling middleware in backend
- **API**: Proper HTTP status codes and error messages

### Success Criteria:
- Errors are caught and displayed clearly
- User can recover from errors
- Data is not lost
- User understands what went wrong

---

## Flow Diagrams Summary

### Primary Flows (Most Common)
1. **Authentication** → **Dashboard** → **View Products** → **Take Action**
2. **Dashboard** → **Low Stock Alert** → **Create Purchase Order**
3. **Products** → **Add Product** → **Save** → **View in List**
4. **Sales** → **Create Sale** → **Update Stock** → **Generate Receipt**

### Secondary Flows (Regular Use)
5. **Reports** → **Select Report** → **Generate** → **Export**
6. **Products** → **Adjust Stock** → **Record Transaction**
7. **Dashboard** → **View Analytics** → **Drill Down**

### Administrative Flows (Occasional)
8. **Admin** → **Manage Users** → **Set Permissions**
9. **Settings** → **Configure System** → **Save**

---

## Navigation Map

```
Login/Register
    ↓
Dashboard (Default)
    ├── Products
    │   ├── View All
    │   ├── Add New
    │   ├── Edit Product
    │   ├── Adjust Stock
    │   └── View Details
    ├── Suppliers
    │   ├── View All
    │   ├── Add Supplier
    │   ├── Create Purchase Order
    │   └── View Orders
    ├── Customers
    │   ├── View All
    │   ├── Add Customer
    │   └── View History
    ├── Transactions
    │   ├── View All
    │   ├── Create Sale
    │   ├── View Details
    │   └── Filter/Search
    ├── Reports
    │   ├── Inventory Reports
    │   ├── Sales Reports
    │   ├── Supplier Reports
    │   └── Custom Reports
    └── Admin (if authorized)
        ├── User Management
        ├── System Settings
        └── Audit Logs
```

---

**Document Version**: 1.0  
**Last Updated**: February 2026  
**Author**: Development Team
