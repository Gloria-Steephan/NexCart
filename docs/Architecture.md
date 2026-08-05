# System Architecture

**Project:** NexCart

**Version:** 1.0

**Status:** Approved

---

# 1. Overview

NexCart follows a modular, feature-based layered architecture that separates business logic, data access, routing, and presentation responsibilities.

The architecture is designed to be:

- Scalable
- Maintainable
- Modular
- Testable
- Secure

The frontend communicates with the backend through REST APIs, while the backend interacts with MongoDB using Mongoose.

---

# 2. High-Level Architecture

```
                ┌────────────────────┐
                │   React Frontend   │
                └─────────┬──────────┘
                          │
                    HTTPS / REST API
                          │
                          ▼
                ┌────────────────────┐
                │ Express Backend API │
                └─────────┬──────────┘
                          │
                Authentication Layer
                          │
                          ▼
                Business Logic Layer
                          │
                          ▼
                 Data Access Layer
                          │
                          ▼
                     MongoDB Atlas
```

---

# 3. Technology Stack

## Frontend

- React
- TypeScript
- Vite
- Tailwind CSS
- React Router
- TanStack Query
- Zustand
- Axios
- React Hook Form
- Zod

---

## Backend

- Node.js
- Express.js
- TypeScript
- Mongoose
- JWT
- Bcrypt

---

## Database

- MongoDB Atlas

---

## Media Storage

- Cloudinary

---

## Deployment

Frontend

- Vercel

Backend

- Render

---

# 4. Architectural Pattern

NexCart uses a **Feature-Based Layered Architecture**.

Each feature contains its own routes, controller, service, repository, model, validation, and types.

Example:

```
Authentication

├── Routes
├── Controller
├── Service
├── Repository
├── Model
├── Validation
└── Types
```

This structure improves modularity and simplifies future maintenance.

---

# 5. Backend Folder Structure

```
server/
│
└── src/
    │
    ├── config/
    ├── middlewares/
    ├── modules/
    │
    │   ├── auth/
    │   ├── users/
    │   ├── products/
    │   ├── categories/
    │   ├── brands/
    │   ├── cart/
    │   ├── wishlist/
    │   ├── addresses/
    │   ├── orders/
    │   ├── payments/
    │   ├── reviews/
    │   ├── coupons/
    │   └── admin/
    │
    ├── shared/
    ├── utils/
    ├── app.ts
    └── server.ts
```

---

# 6. Frontend Folder Structure

```
client/
│
└── src/
    │
    ├── assets/
    ├── components/
    ├── features/
    ├── layouts/
    ├── pages/
    ├── routes/
    ├── services/
    ├── hooks/
    ├── store/
    ├── types/
    ├── utils/
    ├── App.tsx
    └── main.tsx
```

---

# 7. Request Flow

Every API request follows the same execution path.

```
Client
    │
    ▼
Express Route
    │
    ▼
Controller
    │
    ▼
Service
    │
    ▼
Repository
    │
    ▼
MongoDB
```

### Responsibilities

### Routes

- Define API endpoints
- Apply middleware
- Forward requests to controllers

---

### Controllers

Responsible for:

- Receiving HTTP requests
- Calling services
- Returning HTTP responses

Controllers should not contain business logic.

---

### Services

Responsible for:

- Business rules
- Validation
- Processing data
- Calling repositories

---

### Repositories

Responsible for:

- Database queries
- Data persistence
- Data retrieval

---

### Models

Responsible for:

- Database schema definitions
- Validation rules
- Relationships

---

# 8. Authentication Flow

```
User Login

      │
      ▼

Credentials Validation

      │
      ▼

Generate Access Token

      │

Generate Refresh Token

      │
      ▼

Return Tokens

      │
      ▼

Authenticated Requests

      │
      ▼

JWT Verification Middleware

      │
      ▼

Protected Resources
```

---

# 9. API Design Principles

- RESTful architecture
- Resource-oriented endpoints
- API Versioning (`/api/v1`)
- JSON request and response format
- Consistent error handling
- Proper HTTP status codes

---

# 10. Security Architecture

The application implements multiple security layers.

- Password hashing
- JWT authentication
- Refresh tokens
- Role-based authorization
- Input validation
- Environment variables
- CORS protection
- Helmet middleware
- Rate limiting

---

# 11. State Management

## Client State

Managed using:

- Zustand

Examples:

- Authentication
- Theme
- UI state

---

## Server State

Managed using:

- TanStack Query

Examples:

- Products
- Orders
- Categories
- Reviews

---

# 12. Error Handling

A centralized error handler processes all application errors.

Benefits:

- Consistent responses
- Cleaner controllers
- Easier debugging
- Improved maintainability

---

# 13. Logging

The application uses structured logging for:

- Server startup
- API requests
- Errors
- Warnings

Logging improves debugging and production monitoring.

---

# 14. Configuration Management

Sensitive configuration values are stored using environment variables.

Examples:

- Database URL
- JWT Secret
- Refresh Token Secret
- Cloudinary Credentials
- Razorpay Keys

No sensitive values should be hardcoded.

---

# 15. Design Principles

The architecture follows these principles:

- Separation of Concerns
- Single Responsibility Principle
- DRY (Don't Repeat Yourself)
- KISS (Keep It Simple)
- Modular Design
- Reusability
- Scalability

---

# 16. Future Scalability

The architecture supports future enhancements including:

- Google Authentication
- Product Comparison
- AI Recommendations
- Mobile Application
- Email Notifications
- Push Notifications
- Multi-language Support
- Vendor Portal

These features can be added with minimal impact to the existing system.

---

# 17. Architecture Summary

NexCart adopts a modular, feature-based layered architecture that emphasizes clean separation of responsibilities, scalability, maintainability, and security. The system is designed to support future growth while providing a solid foundation for Version 1.0.