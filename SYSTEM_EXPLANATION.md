# Stock Management Control System - Complete System Explanation

## 📋 Table of Contents
1. [System Overview](#system-overview)
2. [Architecture Overview](#architecture-overview)
3. [How the System Works](#how-the-system-works)
4. [Project Structure](#project-structure)
5. [Step-by-Step Development Guide](#step-by-step-development-guide)
6. [Key Components Explained](#key-components-explained)
7. [Data Flow](#data-flow)
8. [API Endpoints](#api-endpoints)
9. [Frontend-Backend Communication](#frontend-backend-communication)

---

## 1. System Overview

### What is Stock Management Control System?

The **Stock Management Control System** is a comprehensive web application designed to help businesses efficiently manage their inventory, track stock levels, monitor suppliers, handle customer orders, and generate detailed reports. It's built as a modern Single Page Application (SPA) with a clear separation between frontend and backend.

### Core Purpose
- **Track Inventory**: Real-time monitoring of product stock levels
- **Manage Suppliers**: Maintain supplier information and purchase orders
- **Handle Customers**: Store customer data and order history
- **Process Transactions**: Record all stock movements (sales, purchases, adjustments)
- **Generate Reports**: Create comprehensive analytics and reports
- **User Management**: Role-based access control for different user types

---

## 2. Architecture Overview

### High-Level Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    CLIENT (Browser)                          │
│  ┌──────────────────────────────────────────────────────┐   │
│  │         Vue.js 3 Frontend (SPA)                      │   │
│  │  • Components (ProductCard, Dashboard, etc.)        │   │
│  │  • Pinia Stores (State Management)                   │   │
│  │  • Vue Router (Navigation)                           │   │
│  │  • Axios (HTTP Client)                               │   │
│  └──────────────────────────────────────────────────────┘   │
└───────────────────────┬─────────────────────────────────────┘
                         │ HTTP/REST API
                         │ (JSON)
                         ▼
┌─────────────────────────────────────────────────────────────┐
│              SERVER (.NET 8.0 Web API)                     │
│  ┌──────────────────────────────────────────────────────┐ │
│  │  Controllers (API Endpoints)                          │ │
│  │  • ProductsController                                 │ │
│  │  • SuppliersController                                │ │
│  │  • CustomersController                                │ │
│  │  • TransactionsController                             │ │
│  │  • ReportsController                                  │ │
│  └──────────────────────────────────────────────────────┘ │
│  ┌──────────────────────────────────────────────────────┐ │
│  │  Services (Business Logic)                            │ │
│  │  • ProductService                                      │ │
│  │  • SupplierService                                     │ │
│  │  • TransactionService                                  │ │
│  └──────────────────────────────────────────────────────┘ │
│  ┌──────────────────────────────────────────────────────┐ │
│  │  Data Layer (Entity Framework Core)                    │ │
│  │  • ApplicationDbContext                                │ │
│  │  • Models (Product, Supplier, Customer, etc.)         │ │
│  └──────────────────────────────────────────────────────┘ │
└───────────────────────┬─────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│                    DATABASE                                 │
│  • SQL Server / PostgreSQL                                  │
│  • Tables: Products, Suppliers, Customers, Transactions     │
└─────────────────────────────────────────────────────────────┘
```

### Technology Stack

**Backend:**
- **.NET 8.0**: Modern, high-performance framework
- **ASP.NET Core Web API**: RESTful API framework
- **Entity Framework Core**: Object-Relational Mapping (ORM)
- **JWT Authentication**: Secure token-based authentication
- **Swagger/OpenAPI**: API documentation

**Frontend:**
- **Vue.js 3**: Progressive JavaScript framework
- **Composition API**: Modern Vue.js component structure
- **Pinia**: State management library
- **Vue Router**: Client-side routing
- **Vite**: Fast build tool
- **Tailwind CSS**: Utility-first CSS framework
- **Axios**: HTTP client for API calls

---

## 3. How the System Works

### 3.1 Single Page Application (SPA) Concept

**Traditional Web App (MPA - Multi-Page Application):**
```
User clicks link → Browser requests new HTML page → Full page reload
```

**Our SPA:**
```
User clicks link → JavaScript updates content → No page reload (faster!)
```

**Benefits:**
- Faster navigation (no full page reloads)
- App-like experience
- Better user experience
- Reduced server load

### 3.2 Component-Based Architecture

The frontend is built using **components** - reusable pieces of UI:

```
App.vue (Root Component)
├── Header.vue (Navigation)
├── Sidebar.vue (Menu)
└── RouterView (Dynamic Content)
    ├── DashboardView.vue (Main Page)
    ├── ProductsView.vue (Product List)
    │   └── ProductCard.vue (Individual Product)
    ├── SuppliersView.vue
    └── ReportsView.vue
```

**Example Component Flow:**
1. User visits `/products`
2. `ProductsView.vue` loads
3. Component calls `productStore.fetchProducts()`
4. Store makes API call: `GET /api/products`
5. Backend returns JSON data
6. Component displays products using `ProductCard.vue`

### 3.3 State Management (Pinia)

**Why State Management?**
Multiple components need access to the same data (e.g., product list).

**How It Works:**
```
┌─────────────────┐
│  Pinia Store    │
│  (Global State) │
└────────┬────────┘
         │
    ┌────┴────┐
    │         │
┌───▼───┐ ┌──▼────┐
│Component│ │Component│
│   A     │ │   B     │
└─────────┘ └─────────┘
```

**Example:**
```javascript
// stores/products.js
export const useProductStore = defineStore('products', {
  state: () => ({
    products: [],
    loading: false
  }),
  
  actions: {
    async fetchProducts() {
      this.loading = true
      const response = await api.get('/products')
      this.products = response.data
      this.loading = false
    }
  }
})
```

### 3.4 API Communication Flow

**Complete Request Flow:**

```
1. User Action (e.g., Click "Add Product")
   ↓
2. Component calls store action
   ↓
3. Store makes HTTP request via Axios
   ↓
4. Request sent to backend API
   ↓
5. Controller receives request
   ↓
6. Controller calls Service
   ↓
7. Service interacts with Database (via EF Core)
   ↓
8. Database returns data
   ↓
9. Service processes data
   ↓
10. Controller returns JSON response
   ↓
11. Store updates state
   ↓
12. Component re-renders with new data
```

**Example: Adding a Product**

```javascript
// Frontend (Vue Component)
async function addProduct() {
  await productStore.addProduct({
    name: "Laptop",
    price: 999.99,
    stock: 10
  })
}

// Store Action
async addProduct(product) {
  const response = await axios.post('/api/products', product)
  this.products.push(response.data)
}

// Backend Controller
[HttpPost]
public async Task<ProductDto> CreateProduct(ProductDto productDto) {
  var product = _mapper.Map<Product>(productDto);
  await _productService.AddProductAsync(product);
  return Ok(product);
}
```

---

## 4. Project Structure

### Backend Structure

```
StockManagementControl/
├── Controllers/              # API Endpoints
│   ├── ProductsController.cs
│   ├── SuppliersController.cs
│   ├── CustomersController.cs
│   ├── TransactionsController.cs
│   └── ReportsController.cs
│
├── Models/                  # Data Models (Database Entities)
│   ├── Product.cs
│   ├── Supplier.cs
│   ├── Customer.cs
│   ├── StockTransaction.cs
│   └── User.cs
│
├── Services/                # Business Logic
│   ├── IProductService.cs
│   ├── ProductService.cs
│   ├── ISupplierService.cs
│   └── SupplierService.cs
│
├── Data/                    # Database Context
│   ├── ApplicationDbContext.cs
│   └── Migrations/
│
├── DTOs/                    # Data Transfer Objects
│   ├── ProductDto.cs
│   ├── CreateProductDto.cs
│   └── UpdateProductDto.cs
│
├── Middleware/              # Custom Middleware
│   └── ErrorHandlingMiddleware.cs
│
├── Program.cs               # Application Entry Point
├── appsettings.json         # Configuration
└── StockManagementControl.csproj
```

### Frontend Structure (Future)

```
frontend/
├── src/
│   ├── main.js              # Entry Point
│   ├── App.vue              # Root Component
│   │
│   ├── router/
│   │   └── index.js         # Route Configuration
│   │
│   ├── stores/              # Pinia Stores
│   │   ├── auth.js
│   │   ├── products.js
│   │   ├── suppliers.js
│   │   └── transactions.js
│   │
│   ├── views/               # Page Components
│   │   ├── DashboardView.vue
│   │   ├── ProductsView.vue
│   │   ├── SuppliersView.vue
│   │   └── ReportsView.vue
│   │
│   ├── components/          # Reusable Components
│   │   ├── layout/
│   │   │   ├── Header.vue
│   │   │   └── Sidebar.vue
│   │   ├── products/
│   │   │   ├── ProductCard.vue
│   │   │   └── ProductForm.vue
│   │   └── common/
│   │       └── LoadingSpinner.vue
│   │
│   ├── services/            # API Service Layer
│   │   └── api.js
│   │
│   └── utils/               # Utility Functions
│       ├── validators.js
│       └── formatters.js
│
└── public/                  # Static Assets
```

---

## 5. Step-by-Step Development Guide

### Phase 1: Backend Foundation ✅ (Current)

#### Step 1.1: Project Setup
```bash
# Create new .NET Web API project
dotnet new webapi -n StockManagementControl

# Navigate to project
cd StockManagementControl

# Restore packages
dotnet restore

# Run the project
dotnet run
```

#### Step 1.2: Database Setup
```bash
# Install Entity Framework tools
dotnet tool install --global dotnet-ef

# Add EF Core packages
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add package Microsoft.EntityFrameworkCore.Design

# Create DbContext
# (Create ApplicationDbContext.cs in Data folder)

# Create initial migration
dotnet ef migrations add InitialCreate

# Apply migration to database
dotnet ef database update
```

#### Step 1.3: Create Models
Create model classes:
- `Product.cs` - Product information
- `Supplier.cs` - Supplier details
- `Customer.cs` - Customer information
- `StockTransaction.cs` - Transaction records
- `User.cs` - User accounts

#### Step 1.4: Create Controllers
Create API controllers:
- `ProductsController.cs` - Product CRUD operations
- `SuppliersController.cs` - Supplier management
- `CustomersController.cs` - Customer management
- `TransactionsController.cs` - Transaction handling

#### Step 1.5: Implement Services
Create service layer:
- `ProductService.cs` - Business logic for products
- `SupplierService.cs` - Business logic for suppliers
- Implement interfaces for testability

### Phase 2: Frontend Development 📅 (Next)

#### Step 2.1: Initialize Vue.js Project
```bash
# Create Vue project with Vite
npm create vue@latest frontend

# Navigate to frontend
cd frontend

# Install dependencies
npm install

# Install additional packages
npm install pinia vue-router axios tailwindcss
```

#### Step 2.2: Setup Routing
```javascript
// router/index.js
import { createRouter, createWebHistory } from 'vue-router'
import DashboardView from '../views/DashboardView.vue'

const routes = [
  { path: '/', component: DashboardView },
  { path: '/products', component: ProductsView },
  // ... more routes
]

const router = createRouter({
  history: createWebHistory(),
  routes
})
```

#### Step 2.3: Setup Pinia Stores
```javascript
// stores/products.js
import { defineStore } from 'pinia'
import api from '../services/api'

export const useProductStore = defineStore('products', {
  state: () => ({
    products: [],
    loading: false
  }),
  
  actions: {
    async fetchProducts() {
      this.loading = true
      const response = await api.get('/products')
      this.products = response.data
      this.loading = false
    }
  }
})
```

#### Step 2.4: Create Components
- Create layout components (Header, Sidebar)
- Create product components (ProductCard, ProductForm)
- Create view components (DashboardView, ProductsView)

### Phase 3: Integration 📅

#### Step 3.1: Connect Frontend to Backend
```javascript
// services/api.js
import axios from 'axios'

const api = axios.create({
  baseURL: 'https://localhost:5001/api',
  headers: {
    'Content-Type': 'application/json'
  }
})

export default api
```

#### Step 3.2: Implement Authentication
- JWT token storage
- Protected routes
- API interceptors for auth headers

### Phase 4: Advanced Features 📅

- Reporting system
- Real-time updates
- Export functionality
- Advanced filtering

---

## 6. Key Components Explained

### 6.1 Product Management Flow

```
┌──────────────┐
│   User       │
│  (Browser)   │
└──────┬───────┘
       │
       │ 1. Clicks "Add Product"
       ▼
┌──────────────────┐
│ ProductsView.vue  │
│  (Component)      │
└──────┬───────────┘
       │
       │ 2. Calls store action
       ▼
┌──────────────────┐
│ productStore     │
│ addProduct()     │
└──────┬───────────┘
       │
       │ 3. HTTP POST request
       ▼
┌──────────────────┐
│ ProductsController│
│  [HttpPost]       │
└──────┬───────────┘
       │
       │ 4. Calls service
       ▼
┌──────────────────┐
│ ProductService    │
│ AddProduct()      │
└──────┬───────────┘
       │
       │ 5. Saves to database
       ▼
┌──────────────────┐
│   Database       │
│   (SQL Server)   │
└──────────────────┘
```

### 6.2 Stock Transaction Flow

**When a sale occurs:**

1. **Frontend**: User clicks "Complete Sale"
2. **Component**: Collects product IDs and quantities
3. **Store**: Calls `transactionStore.createSale()`
4. **API**: `POST /api/transactions/sales`
5. **Controller**: Validates request
6. **Service**: 
   - Creates transaction record
   - Updates product stock levels
   - Records customer purchase
7. **Database**: Saves transaction and updates products
8. **Response**: Returns success with transaction ID
9. **Frontend**: Updates UI, shows receipt

### 6.3 Low Stock Alert System

**How it works:**

1. **Background Job**: Runs periodically (every hour)
2. **Check Products**: Queries all products
3. **Compare**: Current stock vs. minimum threshold
4. **Alert Generation**: Creates alerts for low stock items
5. **Notification**: Updates dashboard in real-time
6. **User Action**: User sees alert, creates purchase order

---

## 7. Data Flow

### 7.1 Reading Data (GET Request)

```
User Request → Component → Store → API → Controller → Service → Database
                                                                     │
Response ← Component ← Store ← API ← Controller ← Service ←────────┘
```

### 7.2 Creating Data (POST Request)

```
User Input → Component → Validation → Store → API → Controller → Service → Database
                                                                              │
Success Response ← Component ← Store ← API ← Controller ← Service ←─────────┘
```

### 7.3 Updating Data (PUT Request)

```
User Edit → Component → Store → API → Controller → Service → Database (Update)
                                                                        │
Updated Data ← Component ← Store ← API ← Controller ← Service ←────────┘
```

---

## 8. API Endpoints

### Products API

```
GET    /api/products              # Get all products
GET    /api/products/{id}         # Get product by ID
POST   /api/products              # Create new product
PUT    /api/products/{id}         # Update product
DELETE /api/products/{id}         # Delete product
GET    /api/products/low-stock    # Get low stock products
```

### Suppliers API

```
GET    /api/suppliers             # Get all suppliers
GET    /api/suppliers/{id}        # Get supplier by ID
POST   /api/suppliers             # Create supplier
PUT    /api/suppliers/{id}        # Update supplier
DELETE /api/suppliers/{id}        # Delete supplier
```

### Transactions API

```
GET    /api/transactions          # Get all transactions
GET    /api/transactions/{id}    # Get transaction by ID
POST   /api/transactions/sales   # Create sale transaction
POST   /api/transactions/purchases # Create purchase transaction
GET    /api/transactions/by-product/{productId} # Get transactions for product
```

---

## 9. Frontend-Backend Communication

### Example: Fetching Products

**Frontend (Vue.js):**
```javascript
// In ProductsView.vue
import { useProductStore } from '@/stores/products'

const productStore = useProductStore()

onMounted(async () => {
  await productStore.fetchProducts()
})
```

**Store (Pinia):**
```javascript
// stores/products.js
async fetchProducts() {
  try {
    this.loading = true
    const response = await api.get('/products')
    this.products = response.data
  } catch (error) {
    console.error('Error fetching products:', error)
  } finally {
    this.loading = false
  }
}
```

**Backend Controller:**
```csharp
[HttpGet]
public async Task<ActionResult<IEnumerable<ProductDto>>> GetProducts()
{
    var products = await _productService.GetAllProductsAsync();
    return Ok(products);
}
```

**Service:**
```csharp
public async Task<IEnumerable<ProductDto>> GetAllProductsAsync()
{
    var products = await _context.Products.ToListAsync();
    return _mapper.Map<IEnumerable<ProductDto>>(products);
}
```

---

## 10. Key Concepts Summary

### Props (Parent → Child)
- Parent component passes data to child
- Example: `ProductsView` passes product data to `ProductCard`

### Emits (Child → Parent)
- Child component sends events to parent
- Example: `ProductCard` emits "delete" event to `ProductsView`

### Lifecycle Hooks
- `onMounted`: Runs when component loads
- Use for fetching initial data

### Async/Await
- Handle operations that take time (API calls)
- Prevents UI freezing

### State Management
- Pinia stores hold global state
- Multiple components can access same data
- Centralized data management

---

## Next Steps

1. **Complete Backend**: Finish all controllers and services
2. **Setup Frontend**: Initialize Vue.js project
3. **Connect**: Link frontend to backend API
4. **Test**: Test all features end-to-end
5. **Deploy**: Deploy to production

---

**Document Version**: 1.0  
**Last Updated**: February 2026  
**Author**: Development Team
