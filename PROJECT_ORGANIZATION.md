# Stock Management Control System - Project Organization Guide

## 📁 Step-by-Step Project Organization

This guide will help you organize your Stock Management Control System project properly in Visual Studio Code.

---

## Step 1: Open Project in Visual Studio Code

### 1.1 Open VS Code Workspace

**Option A: Open via Workspace File**
1. Open Visual Studio Code
2. File → Open Workspace from File...
3. Select `StockManagementControl.code-workspace`
4. Click "Open"

**Option B: Open via Folder**
1. Open Visual Studio Code
2. File → Open Folder...
3. Navigate to: `C:\Users\cyuba\Desktop\WebTech Project\StockManagementControl`
4. Click "Select Folder"

### 1.2 Install Recommended Extensions

VS Code will prompt you to install recommended extensions. Click "Install All" or install manually:

**Essential Extensions:**
- **C# Dev Kit** (`ms-dotnettools.csdevkit`) - Full .NET development support
- **C#** (`ms-dotnettools.csharp`) - C# language support
- **Docker** (`ms-azuretools.vscode-docker`) - Docker support
- **PowerShell** (`ms-vscode.powershell`) - PowerShell support

**Optional but Recommended:**
- **GitLens** - Enhanced Git capabilities
- **Prettier** - Code formatting
- **ESLint** - JavaScript linting (for frontend)

---

## Step 2: Understanding Project Structure

### 2.1 Current Structure

```
StockManagementControl/
├── .vscode/                    # VS Code configuration
│   ├── settings.json          # Editor settings
│   ├── extensions.json        # Recommended extensions
│   ├── launch.json           # Debug configuration
│   └── tasks.json            # Build tasks
│
├── Controllers/               # API Controllers
│   └── WeatherForecastController.cs
│
├── Properties/                # Project properties
│   └── launchSettings.json
│
├── bin/                       # Compiled output (ignored)
├── obj/                       # Build artifacts (ignored)
│
├── Program.cs                 # Application entry point
├── WeatherForecast.cs         # Example model
├── StockManagementControl.csproj  # Project file
├── StockManagementControl.sln     # Solution file
│
├── Documentation/
│   ├── PROJECT_DESCRIPTION.md
│   ├── USER_PERSONAS.md
│   ├── USER_FLOWS.md
│   ├── SYSTEM_EXPLANATION.md
│   └── PROJECT_ORGANIZATION.md (this file)
│
├── .gitignore                 # Git ignore rules
├── Dockerfile                 # Docker configuration
└── README.md                  # Project overview
```

### 2.2 Recommended Folder Structure (To Create)

```
StockManagementControl/
├── Controllers/               # API Controllers
│   ├── ProductsController.cs
│   ├── SuppliersController.cs
│   ├── CustomersController.cs
│   ├── TransactionsController.cs
│   ├── ReportsController.cs
│   └── AuthController.cs
│
├── Models/                    # Data Models (Entities)
│   ├── Product.cs
│   ├── Supplier.cs
│   ├── Customer.cs
│   ├── StockTransaction.cs
│   └── User.cs
│
├── Services/                  # Business Logic Layer
│   ├── Interfaces/
│   │   ├── IProductService.cs
│   │   ├── ISupplierService.cs
│   │   └── ITransactionService.cs
│   │
│   └── Implementations/
│       ├── ProductService.cs
│       ├── SupplierService.cs
│       └── TransactionService.cs
│
├── Data/                      # Database Context
│   ├── ApplicationDbContext.cs
│   └── Migrations/
│
├── DTOs/                      # Data Transfer Objects
│   ├── Products/
│   │   ├── ProductDto.cs
│   │   ├── CreateProductDto.cs
│   │   └── UpdateProductDto.cs
│   │
│   └── Common/
│       └── ApiResponse.cs
│
├── Middleware/                # Custom Middleware
│   ├── ErrorHandlingMiddleware.cs
│   └── AuthenticationMiddleware.cs
│
├── Helpers/                   # Utility Classes
│   ├── Validators.cs
│   └── Formatters.cs
│
└── Tests/                     # Unit Tests (Future)
    ├── Controllers/
    └── Services/
```

---

## Step 3: Organizing Your Code

### 3.1 Create Folder Structure

**Using VS Code:**
1. Right-click in Explorer panel
2. Select "New Folder"
3. Create folders: `Models`, `Services`, `Data`, `DTOs`, `Middleware`

**Using Terminal:**
```powershell
# Navigate to project root
cd "C:\Users\cyuba\Desktop\WebTech Project\StockManagementControl"

# Create folders
mkdir Models
mkdir Services
mkdir Services\Interfaces
mkdir Services\Implementations
mkdir Data
mkdir DTOs
mkdir DTOs\Products
mkdir DTOs\Common
mkdir Middleware
mkdir Helpers
```

### 3.2 Move Existing Files

**Current files to organize:**
- `WeatherForecast.cs` → Move to `Models/` (or delete if not needed)
- `WeatherForecastController.cs` → Keep in `Controllers/` (or delete if not needed)

---

## Step 4: Setting Up Development Environment

### 4.1 Configure VS Code Settings

The `.vscode/settings.json` file is already configured with:
- File exclusions (bin, obj folders)
- Editor formatting
- C# language settings
- Solution file reference

**Verify settings:**
1. Open `.vscode/settings.json`
2. Ensure settings match your preferences
3. Adjust tab size, formatting as needed

### 4.2 Configure Build Tasks

**Available Tasks (defined in `.vscode/tasks.json`):**
- **build** - Build the project
- **watch** - Watch for changes and rebuild
- **clean** - Clean build artifacts
- **restore** - Restore NuGet packages
- **publish** - Publish the project

**Run tasks:**
1. Press `Ctrl+Shift+P` (or `Cmd+Shift+P` on Mac)
2. Type "Tasks: Run Task"
3. Select desired task

### 4.3 Configure Debugging

**Debug Configuration (`.vscode/launch.json`):**
- **.NET Core Launch (web)** - Run and debug the web API
- **.NET Core Attach** - Attach to running process

**Start Debugging:**
1. Press `F5` or click Debug icon
2. Select ".NET Core Launch (web)"
3. Set breakpoints in your code
4. API will start and breakpoints will hit

---

## Step 5: Understanding Key Files

### 5.1 Program.cs

**Purpose:** Application entry point, configures services and middleware

**Key Sections:**
```csharp
// Service registration
builder.Services.AddControllers();
builder.Services.AddSwaggerGen();

// Middleware pipeline
app.UseHttpsRedirection();
app.UseAuthorization();
app.MapControllers();
```

### 5.2 StockManagementControl.csproj

**Purpose:** Project configuration file

**Contains:**
- Target framework (.NET 8.0)
- NuGet package references
- Project settings

### 5.3 appsettings.json

**Purpose:** Application configuration

**Contains:**
- Connection strings
- Logging configuration
- Environment-specific settings

---

## Step 6: Development Workflow

### 6.1 Daily Development Steps

1. **Open Project**
   - Open VS Code
   - Open workspace file

2. **Restore Dependencies**
   ```powershell
   dotnet restore
   ```

3. **Build Project**
   ```powershell
   dotnet build
   ```
   Or use VS Code task: `Ctrl+Shift+P` → "Tasks: Run Task" → "build"

4. **Run Project**
   ```powershell
   dotnet run
   ```
   Or press `F5` to debug

5. **Test API**
   - Open Swagger UI: `https://localhost:5001/swagger`
   - Or use `StockManagementControl.http` file

### 6.2 Adding New Features

**Step-by-Step:**

1. **Create Model** (if needed)
   - Create file in `Models/` folder
   - Define properties

2. **Update DbContext**
   - Add `DbSet<Product>` to `ApplicationDbContext`
   - Create migration: `dotnet ef migrations add AddProducts`

3. **Create DTOs**
   - Create DTOs in `DTOs/` folder
   - Map between Entity and DTO

4. **Create Service Interface**
   - Create interface in `Services/Interfaces/`
   - Define methods

5. **Implement Service**
   - Create implementation in `Services/Implementations/`
   - Implement business logic

6. **Create Controller**
   - Create controller in `Controllers/`
   - Inject service, create endpoints

7. **Register Services**
   - Add to `Program.cs`:
   ```csharp
   builder.Services.AddScoped<IProductService, ProductService>();
   ```

8. **Test**
   - Run project
   - Test via Swagger or HTTP file

---

## Step 7: Git Workflow

### 7.1 Initial Setup

```powershell
# Initialize git (if not already done)
git init

# Add remote (GitLab)
git remote add origin https://gitlab.com/YOUR_USERNAME/stock-management-control.git

# Stage all files
git add .

# Commit
git commit -m "Initial commit: Project setup and documentation"

# Push to GitLab
git push -u origin main
```

### 7.2 Daily Git Workflow

```powershell
# Check status
git status

# Stage changes
git add .

# Commit with message
git commit -m "Add product management feature"

# Push to GitLab
git push
```

### 7.3 Branch Strategy

```powershell
# Create feature branch
git checkout -b feature/product-management

# Work on feature
# ... make changes ...

# Commit changes
git add .
git commit -m "Implement product CRUD operations"

# Push branch
git push -u origin feature/product-management

# Merge to main (via GitLab UI or locally)
git checkout main
git merge feature/product-management
```

---

## Step 8: Testing Your Setup

### 8.1 Verify VS Code Configuration

1. **Check Extensions**
   - Open Extensions panel (`Ctrl+Shift+X`)
   - Verify C# Dev Kit is installed
   - Install any missing recommended extensions

2. **Test Build**
   - Press `Ctrl+Shift+P`
   - Run "Tasks: Run Task" → "build"
   - Check for errors

3. **Test Debug**
   - Set a breakpoint in `Program.cs`
   - Press `F5`
   - Verify breakpoint hits

### 8.2 Verify Project Runs

```powershell
# Restore packages
dotnet restore

# Build project
dotnet build

# Run project
dotnet run
```

**Expected Output:**
```
info: Microsoft.Hosting.Lifetime[14]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[14]
      Now listening on: http://localhost:5000
```

### 8.3 Test API

1. Open browser: `https://localhost:5001/swagger`
2. Test WeatherForecast endpoint
3. Verify response

---

## Step 9: Next Steps

### Immediate Actions:
1. ✅ Open project in VS Code
2. ✅ Install recommended extensions
3. ✅ Verify build works
4. ✅ Create folder structure
5. ✅ Set up GitLab remote

### Development Tasks:
1. Create Product model
2. Create ProductService
3. Create ProductsController
4. Set up database
5. Test CRUD operations

### Future Enhancements:
1. Add authentication
2. Create frontend (Vue.js)
3. Connect frontend to backend
4. Add tests
5. Deploy application

---

## Troubleshooting

### Issue: Extensions not installing
**Solution:** Check internet connection, restart VS Code

### Issue: Build fails
**Solution:** 
```powershell
dotnet clean
dotnet restore
dotnet build
```

### Issue: Debug not working
**Solution:** Check `launch.json` configuration, ensure project builds successfully

### Issue: Swagger not showing
**Solution:** Verify `appsettings.Development.json` has correct environment

---

## Resources

- [.NET Documentation](https://docs.microsoft.com/dotnet/)
- [ASP.NET Core Documentation](https://docs.microsoft.com/aspnet/core)
- [Entity Framework Core](https://docs.microsoft.com/ef/core/)
- [VS Code C# Extension](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)
- [GitLab Documentation](https://docs.gitlab.com/)

---

**Document Version**: 1.0  
**Last Updated**: February 2026
