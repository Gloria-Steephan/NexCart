# Database Design

Project: NexCart

Version: 1.0

Status: Draft

---

# Database Overview

NexCart uses MongoDB as its primary database.

The database is designed using a document-oriented approach while maintaining clear relationships between entities through references where appropriate.

The design prioritizes:

- Scalability
- Data consistency
- Efficient querying
- Maintainability

---

# Collections

Version 1.0 includes the following collections.

1. Users
2. Categories
3. Brands
4. Products
5. Addresses
6. Carts
7. Orders
8. Reviews
9. Coupons

---

# Collection Overview

## Users

Stores customer and administrator accounts.

Fields

- _id
- firstName
- lastName
- email
- password
- phoneNumber
- role
- profileImage
- isEmailVerified
- isBlocked
- refreshToken
- createdAt
- updatedAt

Indexes

- email (Unique)

Relationships

- One User → Many Addresses
- One User → One Cart
- One User → Many Orders
- One User → Many Reviews

---

## Categories

Stores product categories.

Examples

- Smartphones
- Laptops
- Smart Watches
- Accessories

Fields

- _id
- name
- slug
- image
- description
- isActive
- createdAt
- updatedAt

Relationships

- One Category → Many Products

---

## Brands

Stores product brands.

Examples

- Apple
- Samsung
- Sony
- Dell

Fields

- _id
- name
- logo
- description
- isActive
- createdAt
- updatedAt

Relationships

- One Brand → Many Products

---

## Products

Stores all products.

Fields

- _id
- name
- slug
- description
- shortDescription
- category
- brand
- specifications
- variants
- images
- price
- discountPrice
- stock
- rating
- totalReviews
- isFeatured
- isActive
- createdAt
- updatedAt

Indexes

- name
- category
- brand
- price

Relationships

- Many Products → One Category
- Many Products → One Brand
- One Product → Many Reviews

---

## Addresses

Stores customer delivery addresses.

Fields

- _id
- user
- fullName
- phoneNumber
- addressLine1
- addressLine2
- city
- state
- postalCode
- country
- isDefault

Relationships

- Many Addresses → One User

---

## Carts

Stores customer shopping carts.

Fields

- _id
- user
- items
- subtotal
- discount
- shippingCharge
- tax
- totalAmount
- createdAt
- updatedAt

Relationships

- One Cart → One User

---

## Orders

Stores customer orders.

Fields

- _id
- orderNumber
- user
- items
- shippingAddress
- paymentMethod
- paymentStatus
- orderStatus
- subtotal
- discount
- shippingCharge
- tax
- totalAmount
- placedAt
- deliveredAt

Relationships

- Many Orders → One User

---

## Reviews

Stores product reviews.

Fields

- _id
- product
- user
- rating
- title
- comment
- createdAt
- updatedAt

Relationships

- Many Reviews → One Product
- Many Reviews → One User

---

## Coupons

Stores promotional coupons.

Fields

- _id
- code
- description
- discountType
- discountValue
- minimumPurchase
- maximumDiscount
- usageLimit
- expiryDate
- isActive

Indexes

- code (Unique)

Relationships

- Applied during checkout

---

# Relationships

User

├── Addresses

├── Cart

├── Orders

└── Reviews

Category

└── Products

Brand

└── Products

Product

└── Reviews

Order

└── User

---

# MongoDB Design Strategy

The database uses a hybrid approach.

Embedded Documents

- Product Images
- Product Variants
- Product Specifications
- Cart Items
- Order Items

Referenced Documents

- User
- Product
- Category
- Brand
- Review

This minimizes unnecessary joins while keeping frequently accessed data together.

---

# Common Indexes

Users

- email

Products

- name
- category
- brand
- price

Coupons

- code

Orders

- orderNumber

---

# Audit Fields

Every collection includes:

- createdAt
- updatedAt

These fields are automatically maintained.

---

# Future Collections

Reserved for future versions.

- Notifications
- Wishlists (if separated from User)
- Product Comparisons
- Recently Viewed Products
- Loyalty Points
- Return Requests
- Support Tickets