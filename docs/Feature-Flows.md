# Feature Flows

**Project:** NexCart

**Version:** 1.0

**Status:** Approved

---

# 1. Purpose

This document defines the end-to-end flow of the major features in NexCart.

Each feature describes how an action moves through the system, from the user interface to the backend, business logic, database, external services, and finally back to the user interface.

The purpose of this document is to provide a clear implementation blueprint before development begins.

---

# 2. General Feature Architecture

All NexCart features follow the following general flow:

```text
User Action
    ↓
React UI
    ↓
Client-side Validation
    ↓
Axios
    ↓
REST API
    ↓
Express Route
    ↓
Authentication / Authorization
    ↓
Request Validation
    ↓
Controller
    ↓
Service
    ↓
Repository
    ↓
MongoDB
    ↓
Repository Response
    ↓
Service
    ↓
Controller
    ↓
API Response
    ↓
TanStack Query / Zustand
    ↓
React UI Update
```

---

# 3. Authentication

## 3.1 User Registration

### Flow

```text
User
    ↓
Registration Page
    ↓
Enter Account Information
    ↓
Client-side Validation
    ↓
POST /api/v1/auth/register
    ↓
Request Validation
    ↓
Auth Controller
    ↓
Auth Service
    ↓
Check Existing Email
    ↓
Hash Password
    ↓
Create User
    ↓
User Repository
    ↓
MongoDB
    ↓
Registration Response
    ↓
React UI
    ↓
Display Success
    ↓
Redirect to Login
```

### Database Operations

```text
Users Collection
    ↓
Check Email
    ↓
Create User
```

### Business Rules

- Email must be unique.
- Password must satisfy password requirements.
- Password must never be stored in plain text.
- New users receive the `CUSTOMER` role by default.
- New accounts are initially unverified if email verification is enabled.

---

## 3.2 User Login

### Flow

```text
User
    ↓
Login Page
    ↓
Enter Email & Password
    ↓
Client-side Validation
    ↓
POST /api/v1/auth/login
    ↓
Request Validation
    ↓
Auth Controller
    ↓
Auth Service
    ↓
Find User
    ↓
Compare Password
    ↓
Generate Access Token
    ↓
Generate Refresh Token
    ↓
Store / Update Refresh Token
    ↓
Return Authentication Response
    ↓
Frontend
    ↓
Establish Authenticated Session
    ↓
Redirect User
```

### Business Rules

- Invalid credentials must not reveal whether the email exists.
- Blocked users cannot authenticate.
- Access tokens have a limited lifetime.
- Refresh tokens are used to obtain new access tokens.

---

## 3.3 Logout

### Flow

```text
User
    ↓
Logout
    ↓
POST /api/v1/auth/logout
    ↓
Authentication Middleware
    ↓
Auth Controller
    ↓
Auth Service
    ↓
Invalidate Refresh Token
    ↓
Return Success
    ↓
Clear Client Authentication State
    ↓
Redirect to Public Area
```

---

## 3.4 Forgot / Reset Password

### Flow

```text
User
    ↓
Forgot Password
    ↓
Enter Email
    ↓
POST /api/v1/auth/forgot-password
    ↓
Validate Request
    ↓
Find Account
    ↓
Generate Password Reset Token
    ↓
Store Token / Expiry
    ↓
Send Reset Link
    ↓
User Opens Reset Link
    ↓
Enter New Password
    ↓
POST /api/v1/auth/reset-password
    ↓
Validate Token
    ↓
Hash New Password
    ↓
Update User
    ↓
Invalidate Reset Token
    ↓
Return Success
```

---

# 4. User Profile

## Flow

```text
User
    ↓
Profile Page
    ↓
GET /api/v1/users/profile
    ↓
Authentication Middleware
    ↓
User Controller
    ↓
User Service
    ↓
User Repository
    ↓
MongoDB
    ↓
Return User Profile
    ↓
React UI
```

### Update Profile

```text
Edit Profile
    ↓
Client-side Validation
    ↓
PUT /api/v1/users/profile
    ↓
Authentication
    ↓
Validation
    ↓
User Controller
    ↓
User Service
    ↓
User Repository
    ↓
MongoDB
    ↓
Return Updated Profile
    ↓
Update UI
```

---

# 5. Product Catalog

## 5.1 Browse Products

### Flow

```text
User
    ↓
Homepage / Product Listing
    ↓
Request Products
    ↓
GET /api/v1/products
    ↓
Product Controller
    ↓
Product Service
    ↓
Product Repository
    ↓
MongoDB
    ↓
Apply Filters / Sorting / Pagination
    ↓
Return Products
    ↓
TanStack Query
    ↓
Product Grid
```

### Database Operations

```text
Products Collection
    ↓
Find Active Products
    ↓
Apply Query
    ↓
Pagination
    ↓
Return Results
```

---

## 5.2 Product Details

### Flow

```text
User
    ↓
Click Product
    ↓
Product Details Page
    ↓
GET /api/v1/products/:id
    ↓
Product Controller
    ↓
Product Service
    ↓
Product Repository
    ↓
MongoDB
    ↓
Return Product
    ↓
Fetch Product Reviews
    ↓
Fetch Related Products
    ↓
Render Product Page
```

---

## 5.3 Search

### Flow

```text
User
    ↓
Search Bar
    ↓
Enter Search Term
    ↓
Debounce Input
    ↓
GET /api/v1/products?search=<query>
    ↓
Product Controller
    ↓
Search / Product Service
    ↓
Product Repository
    ↓
MongoDB
    ↓
Search Query
    ↓
Return Matching Products
    ↓
Update Product Grid
```

---

## 5.4 Product Filtering

### Flow

```text
User
    ↓
Select Category / Brand / Price / Rating
    ↓
Build Query Parameters
    ↓
GET /api/v1/products
    ↓
Product Controller
    ↓
Product Service
    ↓
Build Database Query
    ↓
Product Repository
    ↓
MongoDB
    ↓
Return Filtered Products
    ↓
Update Product Grid
```

---

## 5.5 Product Sorting

### Flow

```text
User
    ↓
Select Sort Option
    ↓
Build Sort Parameter
    ↓
GET /api/v1/products?sort=<option>
    ↓
Product Service
    ↓
Product Repository
    ↓
MongoDB
    ↓
Sorted Results
    ↓
Update Product Grid
```

---

# 6. Wishlist

## Add Product

```text
User
    ↓
Click Wishlist
    ↓
Check Authentication
    ↓
POST /api/v1/wishlist
    ↓
Authentication Middleware
    ↓
Wishlist Controller
    ↓
Wishlist Service
    ↓
Validate Product
    ↓
Add Product Reference
    ↓
MongoDB
    ↓
Return Wishlist
    ↓
Update Wishlist UI
```

## Remove Product

```text
User
    ↓
Click Wishlist Icon
    ↓
DELETE /api/v1/wishlist/:productId
    ↓
Authentication
    ↓
Wishlist Service
    ↓
Remove Product Reference
    ↓
MongoDB
    ↓
Return Updated Wishlist
    ↓
Update UI
```

---

# 7. Shopping Cart

## 7.1 Add Product to Cart

### Flow

```text
User
    ↓
Product Details
    ↓
Select Variant
    ↓
Select Quantity
    ↓
Click Add to Cart
    ↓
POST /api/v1/cart/items
    ↓
Authentication Middleware
    ↓
Request Validation
    ↓
Cart Controller
    ↓
Cart Service
    ↓
Validate Product
    ↓
Validate Variant
    ↓
Check Inventory
    ↓
Find User Cart
    ↓
Check Existing Cart Item
    ↓
Update Quantity OR Add Item
    ↓
Cart Repository
    ↓
MongoDB
    ↓
Return Updated Cart
    ↓
Update Cart State
    ↓
Update Cart Badge
```

---

## 7.2 Update Cart Quantity

```text
User
    ↓
Increase / Decrease Quantity
    ↓
PUT /api/v1/cart/items/:id
    ↓
Authentication
    ↓
Validation
    ↓
Cart Service
    ↓
Validate Inventory
    ↓
Update Quantity
    ↓
MongoDB
    ↓
Return Updated Cart
    ↓
Update UI
```

---

## 7.3 Remove Cart Item

```text
User
    ↓
Remove Item
    ↓
DELETE /api/v1/cart/items/:id
    ↓
Authentication
    ↓
Cart Service
    ↓
Remove Item
    ↓
MongoDB
    ↓
Return Updated Cart
    ↓
Update UI
```

---

# 8. Address Management

## Add Address

```text
User
    ↓
Address Book
    ↓
Add Address
    ↓
Fill Address Form
    ↓
Client-side Validation
    ↓
POST /api/v1/addresses
    ↓
Authentication
    ↓
Validation
    ↓
Address Controller
    ↓
Address Service
    ↓
Address Repository
    ↓
MongoDB
    ↓
Return Address
    ↓
Update Address List
```

## Update / Delete Address

```text
Address Book
    ↓
Select Address
    ↓
Edit / Delete
    ↓
API Request
    ↓
Authentication
    ↓
Address Service
    ↓
Repository
    ↓
MongoDB
    ↓
Return Updated Address List
```

---

# 9. Checkout

## Flow

```text
Customer
    ↓
Cart
    ↓
Click Checkout
    ↓
Validate Cart
    ↓
Select Delivery Address
    ↓
Apply Coupon
    ↓
Calculate Order Summary
    ↓
Select Payment Method
    ↓
Review Order
    ↓
Place Order
```

### Backend Checkout Flow

```text
POST /api/v1/checkout
    ↓
Authentication
    ↓
Validate Cart
    ↓
Validate Products
    ↓
Validate Variants
    ↓
Validate Inventory
    ↓
Validate Address
    ↓
Validate Coupon
    ↓
Calculate Subtotal
    ↓
Calculate Discount
    ↓
Calculate Shipping
    ↓
Calculate Tax
    ↓
Calculate Final Amount
    ↓
Proceed to Payment / Order Creation
```

---

# 10. Coupon

## Customer Flow

```text
Checkout
    ↓
Enter Coupon Code
    ↓
POST /api/v1/coupons/apply
    ↓
Coupon Service
    ↓
Find Coupon
    ↓
Check Active Status
    ↓
Check Expiry
    ↓
Check Usage Limit
    ↓
Check Minimum Purchase
    ↓
Calculate Discount
    ↓
Return Discount
    ↓
Update Checkout Total
```

### Business Rules

- Coupon must be active.
- Coupon must not be expired.
- Minimum purchase requirements must be satisfied.
- Usage limits must be respected.
- Maximum discount must be respected where applicable.

---

# 11. Order Placement

## General Flow

```text
Customer
    ↓
Place Order
    ↓
Validate Cart
    ↓
Validate Inventory
    ↓
Validate Address
    ↓
Calculate Final Amount
    ↓
Payment Processing
    ↓
Create Order
    ↓
Create Order Items
    ↓
Store Product Snapshots
    ↓
Store Address Snapshot
    ↓
Update Inventory
    ↓
Clear Cart
    ↓
Return Order
    ↓
Order Confirmation Page
```

### Database Flow

```text
Orders Collection
    ↓
Create Order
    ↓
Store Order Items
    ↓
Store Product Information Snapshot
    ↓
Store Shipping Address Snapshot
    ↓
Store Payment Information
    ↓
Save Order
```

---

# 12. Cash on Delivery

## Flow

```text
Checkout
    ↓
Select Cash on Delivery
    ↓
Review Order
    ↓
Place Order
    ↓
Validate Cart / Inventory
    ↓
Create Order
    ↓
Payment Method = COD
    ↓
Payment Status = Pending
    ↓
Order Status = Pending
    ↓
Update Inventory
    ↓
Clear Cart
    ↓
Order Confirmation
```

---

# 13. Razorpay Payment

## Flow

```text
Checkout
    ↓
Select Razorpay
    ↓
Request Payment Creation
    ↓
POST /api/v1/payments/create
    ↓
Backend Creates Razorpay Order
    ↓
Return Payment Details
    ↓
Open Razorpay Checkout
    ↓
Customer Completes Payment
    ↓
Payment Result
    ↓
POST /api/v1/payments/verify
    ↓
Backend Verifies Payment
    ↓
Update Payment Status
    ↓
Create / Confirm Order
    ↓
Update Inventory
    ↓
Clear Cart
    ↓
Order Confirmation
```

---

# 14. Order History

## Flow

```text
Customer
    ↓
Orders Page
    ↓
GET /api/v1/orders
    ↓
Authentication
    ↓
Order Controller
    ↓
Order Service
    ↓
Order Repository
    ↓
MongoDB
    ↓
Return Customer Orders
    ↓
Display Order List
```

---

# 15. Order Details

## Flow

```text
Customer
    ↓
Select Order
    ↓
GET /api/v1/orders/:id
    ↓
Authentication
    ↓
Verify Order Ownership
    ↓
Order Service
    ↓
Repository
    ↓
MongoDB
    ↓
Return Order Details
    ↓
Display Order Details
```

---

# 16. Order Tracking

## Flow

```text
Customer
    ↓
Order Details
    ↓
GET /api/v1/orders/:id/track
    ↓
Authentication
    ↓
Order Service
    ↓
Return Current Order Status
    ↓
Display Tracking Timeline
```

## Order Status Flow

```text
Pending
    ↓
Confirmed
    ↓
Packed
    ↓
Shipped
    ↓
Out for Delivery
    ↓
Delivered
```

## Cancellation Flow

```text
Pending / Confirmed
    ↓
Customer Requests Cancellation
    ↓
PATCH /api/v1/orders/:id/cancel
    ↓
Validate Cancellation Eligibility
    ↓
Cancel Order
    ↓
Restore Inventory
    ↓
Update Payment / Refund State if Required
    ↓
Return Updated Order
```

---

# 17. Product Reviews

## Flow

```text
Customer
    ↓
Delivered Order
    ↓
Select Purchased Product
    ↓
Write Review
    ↓
Enter Rating
    ↓
Enter Comment
    ↓
POST /api/v1/products/:id/reviews
    ↓
Authentication
    ↓
Validate Product
    ↓
Verify Purchase
    ↓
Verify Delivered Status
    ↓
Check Existing Review
    ↓
Create Review
    ↓
Update Product Rating
    ↓
Return Review
    ↓
Display Review
```

### Business Rules

- Only authenticated customers can review.
- Customer must have purchased the product.
- Order must be delivered.
- A customer cannot submit duplicate reviews for the same purchase/product.

---

# 18. Admin Authentication & Authorization

## Flow

```text
Administrator
    ↓
Admin Login
    ↓
POST /api/v1/auth/login
    ↓
Authenticate User
    ↓
Verify Role
    ↓
Generate Tokens
    ↓
Authenticated Admin Session
    ↓
Access Administration Panel
```

### Protected Admin Request

```text
Admin Request
    ↓
JWT Authentication
    ↓
Role Authorization
    ↓
Request Validation
    ↓
Controller
    ↓
Service
    ↓
Repository
    ↓
MongoDB
```

---

# 19. Admin Product Management

## Add Product

```text
Admin
    ↓
Administration Panel
    ↓
Products
    ↓
Add Product
    ↓
Enter Product Information
    ↓
Add Category / Brand
    ↓
Add Specifications
    ↓
Add Variants
    ↓
Upload Images
    ↓
Submit
    ↓
POST /api/v1/admin/products
    ↓
Authentication
    ↓
Admin Authorization
    ↓
Validation
    ↓
Upload Images to Cloudinary
    ↓
Receive Image URLs
    ↓
Product Service
    ↓
Product Repository
    ↓
MongoDB
    ↓
Product Created
```

---

# 20. Admin Product Update

```text
Admin
    ↓
Products
    ↓
Select Product
    ↓
Edit Product
    ↓
Update Information
    ↓
PUT /api/v1/admin/products/:id
    ↓
Authentication
    ↓
Admin Authorization
    ↓
Validation
    ↓
Product Service
    ↓
Product Repository
    ↓
MongoDB
    ↓
Return Updated Product
    ↓
Update Admin UI
```

---

# 21. Admin Inventory Management

## Flow

```text
Admin
    ↓
Administration Panel
    ↓
Inventory
    ↓
Select Product
    ↓
Select Variant
    ↓
Update Stock
    ↓
PATCH /api/v1/admin/products/:id/inventory
    ↓
Authentication
    ↓
Admin Authorization
    ↓
Validate Product / Variant
    ↓
Inventory Service
    ↓
Update Stock
    ↓
MongoDB
    ↓
Return Updated Inventory
    ↓
Update Admin UI
```

## Inventory States

```text
In Stock
    ↓
Low Stock
    ↓
Out of Stock
```

---

# 22. Admin Order Management

## Flow

```text
Customer Places Order
    ↓
Order Appears in Administration Panel
    ↓
Admin Opens Order
    ↓
Review Order
    ↓
Confirm Order
    ↓
Pack Order
    ↓
Ship Order
    ↓
Mark Out for Delivery
    ↓
Mark Delivered
    ↓
Customer Order Status Updated
```

## Backend Flow

```text
PATCH /api/v1/admin/orders/:id
    ↓
Authentication
    ↓
Admin Authorization
    ↓
Validate Status Transition
    ↓
Order Service
    ↓
Order Repository
    ↓
MongoDB
    ↓
Return Updated Order
```

---

# 23. Admin Customer Management

## Flow

```text
Admin
    ↓
Customers
    ↓
View Customer List
    ↓
Select Customer
    ↓
View Customer Details
    ↓
Block / Unblock Customer
    ↓
PATCH /api/v1/admin/users/:id/block
    ↓
Authentication
    ↓
Admin Authorization
    ↓
User Service
    ↓
Update Account Status
    ↓
MongoDB
    ↓
Return Updated User
```

---

# 24. Admin Category Management

## Add Category

```text
Admin
    ↓
Categories
    ↓
Add Category
    ↓
Enter Category Information
    ↓
Validate
    ↓
POST /api/v1/admin/categories
    ↓
Category Service
    ↓
Category Repository
    ↓
MongoDB
    ↓
Category Created
```

## Update / Delete Category

```text
Admin
    ↓
Select Category
    ↓
Edit / Deactivate
    ↓
API Request
    ↓
Authorization
    ↓
Category Service
    ↓
Repository
    ↓
MongoDB
    ↓
Update UI
```

---

# 25. Admin Brand Management

## Flow

```text
Admin
    ↓
Brands
    ↓
Add / Edit Brand
    ↓
Validate Information
    ↓
API Request
    ↓
Authentication
    ↓
Admin Authorization
    ↓
Brand Service
    ↓
Brand Repository
    ↓
MongoDB
    ↓
Update Brand List
```

---

# 26. Admin Coupon Management

## Create Coupon

```text
Admin
    ↓
Coupons
    ↓
Create Coupon
    ↓
Enter Coupon Rules
    ↓
Validate
    ↓
POST /api/v1/admin/coupons
    ↓
Coupon Service
    ↓
Coupon Repository
    ↓
MongoDB
    ↓
Coupon Created
```

---

# 27. Admin Dashboard & Analytics

## Flow

```text
Admin
    ↓
Dashboard
    ↓
Request Analytics
    ↓
GET /api/v1/admin/dashboard
    ↓
Authentication
    ↓
Admin Authorization
    ↓
Analytics Controller
    ↓
Analytics Service
    ↓
Repository / Aggregation Queries
    ↓
MongoDB
    ↓
Calculate Metrics
    ↓
Return Analytics
    ↓
TanStack Query
    ↓
Dashboard UI
```

## Dashboard Data

- Total Revenue
- Total Orders
- Total Customers
- Total Products
- Sales Trends
- Order Trends
- Top Selling Products
- Low Stock Products

---

# 28. Product Image Management

## Flow

```text
Admin
    ↓
Product Form
    ↓
Select Images
    ↓
Frontend Upload
    ↓
Backend
    ↓
Cloudinary
    ↓
Cloudinary Returns URLs
    ↓
Product Service
    ↓
Store Image URLs in Product
    ↓
MongoDB
```

---

# 29. Error Handling Flow

All feature errors follow a centralized flow.

```text
Request
    ↓
Route
    ↓
Controller
    ↓
Service
    ↓
Error Occurs
    ↓
Error Propagation
    ↓
Global Error Handler
    ↓
Standard Error Response
    ↓
Frontend
    ↓
Display User-Friendly Error
```

## Standard Error Response

```json
{
  "success": false,
  "message": "Unable to complete the request.",
  "errors": []
}
```

---

# 30. External Service Integration

## Cloudinary

```text
React
    ↓
Backend
    ↓
Cloudinary
    ↓
Image URL
    ↓
MongoDB
```

Cloudinary is responsible for media storage while MongoDB stores the resulting media URLs.

---

## Razorpay

```text
React Checkout
    ↓
Backend
    ↓
Razorpay
    ↓
Customer Payment
    ↓
Payment Verification
    ↓
Backend
    ↓
MongoDB
    ↓
Order Confirmation
```

---

# 31. Complete NexCart Feature Flow

The complete customer purchasing journey is:

```text
Landing Page
    ↓
Browse Products
    ↓
Search / Filter / Sort
    ↓
Product Details
    ↓
Select Variant
    ↓
Add to Cart
    ↓
Cart
    ↓
Checkout
    ↓
Select Address
    ↓
Apply Coupon
    ↓
Select Payment Method
    ↓
Payment / COD
    ↓
Order Creation
    ↓
Inventory Update
    ↓
Cart Cleared
    ↓
Order Confirmation
    ↓
Order Tracking
    ↓
Delivery
    ↓
Product Review
```

---

# 32. Feature Flow Design Principles

All NexCart features must follow these principles:

- Frontend should handle presentation and client-side interaction.
- Controllers should handle HTTP requests and responses.
- Services should contain business logic.
- Repositories should handle database access.
- Authentication must be applied to protected resources.
- Authorization must be applied to administrative resources.
- Input must be validated before business logic executes.
- Database operations must maintain data consistency.
- External services must be isolated behind dedicated service logic.
- API responses must follow the standard response format.
- Errors must be handled centrally.
- Server state should be managed using TanStack Query.
- Client-side application state should be managed using Zustand.
- Feature flows should remain consistent across the application.