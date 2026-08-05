# Software Requirements Specification (SRS)

# NexCart

Version: 1.0

Status: Draft

---

# 1. Introduction

## 1.1 Purpose

This Software Requirements Specification (SRS) defines the functional and non-functional requirements for NexCart. It serves as the primary reference for designing, developing, testing, and maintaining the application.

---

## 1.2 Scope

NexCart is a single-vendor e-commerce platform for consumer electronics. The platform enables customers to browse products, place orders, manage their accounts, and track purchases while providing administrators with comprehensive tools to manage products, inventory, orders, customers, and business operations.

---

## 1.3 Intended Audience

This document is intended for:

- Product Owner
- Frontend Developers
- Backend Developers
- UI/UX Designers
- QA Engineers
- Future Contributors

---

# 2. User Roles

## Guest

- Browse products
- Search products
- View product details
- Register
- Login

---

## Customer

- Manage profile
- Manage addresses
- Add products to cart
- Manage wishlist
- Place orders
- Track orders
- Write reviews

---

## Administrator

- Manage products
- Manage categories
- Manage brands
- Manage inventory
- Manage users
- Manage orders
- Manage coupons
- View analytics

---

# 3. Functional Requirements

## Authentication

FR-001 User registration

FR-002 User login

FR-003 User logout

FR-004 Forgot password

FR-005 Reset password

FR-006 JWT authentication

FR-007 Role-based authorization

---

## Product Catalog

FR-008 Browse products

FR-009 View product details

FR-010 Search products

FR-011 Filter products

FR-012 Sort products

FR-013 View product specifications

---

## Shopping Cart

FR-014 Add product to cart

FR-015 Update cart quantity

FR-016 Remove product from cart

FR-017 View cart summary

---

## Wishlist

FR-018 Add product to wishlist

FR-019 Remove product from wishlist

FR-020 Move wishlist item to cart

---

## Address Management

FR-021 Add address

FR-022 Edit address

FR-023 Delete address

FR-024 Set default address

---

## Checkout

FR-025 Select shipping address

FR-026 Apply coupon

FR-027 Select payment method

FR-028 Place order

---

## Orders

FR-029 View order history

FR-030 View order details

FR-031 Cancel order

FR-032 Track order status

---

## Payments

FR-033 Cash on Delivery

FR-034 Razorpay Test Mode

---

## Reviews

FR-035 Add review

FR-036 Edit review

FR-037 Delete review

FR-038 Rate product

---

## Administration

FR-039 Dashboard overview

FR-040 Product management

FR-041 Category management

FR-042 Brand management

FR-043 Inventory management

FR-044 Order management

FR-045 Customer management

FR-046 Coupon management

---

# 4. Non-Functional Requirements

## Performance

- API response time should remain responsive under normal load.
- Product listing should support pagination.
- Images should be optimized for web delivery.

---

## Security

- Passwords must be securely hashed.
- Protected routes require authentication.
- Role-based access control must be enforced.
- User input must be validated.
- Sensitive configuration must be stored using environment variables.

---

## Reliability

- The application should provide consistent error handling.
- Invalid requests should return meaningful error messages.

---

## Scalability

- The application should follow a modular architecture.
- Business logic should remain independent of presentation logic.

---

## Usability

- Responsive design for desktop, tablet, and mobile devices.
- Consistent user interface across all pages.

---

# 5. Assumptions

- Users have internet connectivity.
- Products are managed by administrators.
- Payments use supported payment methods.
- Inventory is maintained by administrators.

---

# 6. Constraints

- Single-vendor platform.
- Consumer electronics domain.
- Google Authentication is excluded from Version 1.0.
- Razorpay operates in Test Mode for Version 1.0.

---

# 7. Dependencies

Frontend

- React
- TypeScript
- Tailwind CSS

Backend

- Node.js
- Express.js

Database

- MongoDB

Deployment

- Vercel
- Render
- MongoDB Atlas