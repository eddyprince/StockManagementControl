# StockManagementControl

A modern, full-stack web application for efficient inventory and stock management, built with .NET 8.0 Web API backend and Vue.js 3 frontend.

> **Note**: This project is a Web Technologies assignment inspired by the TaskBuddy project architecture and best practices.

## 📚 Documentation

This project follows modern web development best practices and includes comprehensive documentation:

- **[PROJECT_DESCRIPTION.md](./PROJECT_DESCRIPTION.md)** - Complete project overview, architecture, features, and technical specifications
- **[USER_PERSONAS.md](./USER_PERSONAS.md)** - Detailed user personas representing different user types and their needs
- **[USER_FLOWS.md](./USER_FLOWS.md)** - Step-by-step user journeys through the application

## 🚀 Quick Start

### Prerequisites
- .NET 8.0 SDK
- Node.js 18+ and npm/yarn
- SQL Server or PostgreSQL
- Docker (optional, for containerized deployment)

### Backend Setup
```bash
# Restore dependencies
dotnet restore

# Run database migrations
dotnet ef database update

# Run the API
dotnet run
```

The API will be available at `https://localhost:5001` (or configured port).

### Frontend Setup
```bash
# Navigate to frontend directory
cd frontend

# Install dependencies
npm install

# Start development server
npm run dev
```

The frontend will be available at `http://localhost:5173` (Vite default port).

## 🏗️ Project Structure

```
StockManagementControl/
├── Backend (ASP.NET Core Web API)
│   ├── Controllers/      # API endpoints
│   ├── Models/           # Data models
│   ├── Services/         # Business logic
│   ├── Data/             # Database context
│   └── DTOs/             # Data transfer objects
│
└── Frontend (Vue.js 3)
    ├── src/
    │   ├── views/        # Page components
    │   ├── components/   # Reusable components
    │   ├── stores/       # Pinia state management
    │   ├── router/       # Vue Router configuration
    │   └── services/     # API service layer
    └── public/           # Static assets
```

## ✨ Key Features

- **Inventory Management**: Real-time stock tracking with low stock alerts
- **Supplier Management**: Track suppliers and purchase orders
- **Customer Management**: Maintain customer database and order history
- **Transaction Tracking**: Complete audit trail of all stock movements
- **Reporting & Analytics**: Comprehensive reports with export capabilities
- **Role-Based Access Control**: Secure multi-user system with permissions
- **Responsive Design**: Mobile-friendly interface built with Tailwind CSS

## 🎯 Target Users

- **Store Managers**: Daily inventory operations
- **Warehouse Supervisors**: Stock receiving and distribution
- **Business Owners**: Strategic insights and reporting
- **Inventory Staff**: Quick stock updates
- **IT Administrators**: User and system management
- **Accountants**: Financial reporting and data export

## 🛠️ Technology Stack

### Backend
- .NET 8.0
- ASP.NET Core Web API
- Entity Framework Core
- JWT Authentication
- Swagger/OpenAPI

### Frontend
- Vue.js 3 (Composition API)
- Vite
- Pinia (State Management)
- Vue Router
- Tailwind CSS
- Axios

## 📋 Development Roadmap

### Phase 1: Foundation ✅
- Project setup and structure
- Authentication system
- Basic CRUD operations

### Phase 2: Core Features 🚧
- Product management
- Supplier management
- Customer management
- Transaction system

### Phase 3: Advanced Features 📅
- Reporting system
- Analytics dashboard
- Export functionality
- Multi-location support

### Phase 4: Polish & Security 📅
- Accessibility improvements
- Security hardening
- Performance optimization
- Comprehensive testing

## 🔒 Security

- JWT-based authentication
- Role-based access control (RBAC)
- Input validation and sanitization
- XSS prevention
- SQL injection prevention
- CORS configuration
- Rate limiting

## ♿ Accessibility

- WCAG 2.1 AA compliance target
- Semantic HTML
- Keyboard navigation support
- Screen reader compatibility
- Focus indicators
- Color contrast compliance

## 📖 Learning Resources

This project is designed as a learning tool for modern web development. Key concepts covered:

- **Single Page Applications (SPA)**: Client-side routing and navigation
- **Component-Based Architecture**: Reusable Vue components
- **State Management**: Pinia stores for global state
- **RESTful APIs**: Backend API design and implementation
- **Authentication & Authorization**: Secure user management
- **Responsive Design**: Mobile-first approach with Tailwind CSS

## 🤝 Contributing

This is an educational project. Contributions and improvements are welcome!

## 📄 License

This project is created for educational purposes as part of the Web Technologies & Modern Frontend Architectures course.

## 📞 Support

For questions or issues, please refer to the documentation files or contact the development team.

---

**Built with ❤️ for modern web development education**
