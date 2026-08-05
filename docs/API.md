# API Design

**Project:** NexCart

**Version:** 1.0

**Status:** Draft

---

# Overview

NexCart exposes RESTful APIs that enable communication between the frontend application and the backend services.

## Base URL

```
/api/v1
```

---

# API Standards

## Request Format

Requests should use JSON unless otherwise specified.

Example:

```json
{
    "email": "john@example.com",
    "password": "password123"
}
```

---

## Success Response

```json
{
    "success": true,
    "message": "Request completed successfully.",
    "data": {}
}
```

---

## Error Response

```json
{
    "success": false,
    "message": "Validation failed.",
    "errors": []
}
```

---

# Authentication APIs

## Register User

POST `/auth/register`

Description

Registers a new customer account.

Authentication

Not Required

---

## Login

POST `/auth/login`

Description

Authenticates a user and returns access and refresh tokens.

Authentication

Not Required

---

## Logout

POST `/auth/logout`

Authentication

Required

---

## Refresh Token

POST `/auth/refresh-token`

Authentication

Required

---

## Forgot Password

POST `/auth/forgot-password`

Authentication

Not Required

---

## Reset Password

POST `/auth/reset-password`

Authentication

Not Required

---

## Get Profile

GET `/users/profile`

Authentication

Required

---

## Update Profile

PUT `/users/profile`

Authentication

Required

---

# Category APIs

## Get Categories

GET `/categories`

---

## Get Category

GET `/categories/:id`

---

# Brand APIs

## Get Brands

GET `/brands`

---

## Get Brand

GET `/brands/:id`

---

# Product APIs

## Get Products

GET `/products`

Supports

- Search
- Filters
- Sorting
- Pagination

---

## Get Product

GET `/products/:id`

---

## Get Featured Products

GET `/products/featured`

---

## Search Products

GET `/products/search`

---

# Cart APIs

## Get Cart

GET `/cart`

Authentication Required

---

## Add Item

POST `/cart/items`

---

## Update Quantity

PUT `/cart/items/:id`

---

## Remove Item

DELETE `/cart/items/:id`

---

## Clear Cart

DELETE `/cart`

---

# Wishlist APIs

## Get Wishlist

GET `/wishlist`

---

## Add Product

POST `/wishlist`

---

## Remove Product

DELETE `/wishlist/:productId`

---

# Address APIs

## Get Addresses

GET `/addresses`

---

## Add Address

POST `/addresses`

---

## Update Address

PUT `/addresses/:id`

---

## Delete Address

DELETE `/addresses/:id`

---

# Checkout APIs

## Checkout

POST `/checkout`

---

# Order APIs

## Place Order

POST `/orders`

---

## Get Orders

GET `/orders`

---

## Get Order Details

GET `/orders/:id`

---

## Cancel Order

PATCH `/orders/:id/cancel`

---

## Track Order

GET `/orders/:id/track`

---

# Payment APIs

## Create Payment

POST `/payments/create`

---

## Verify Payment

POST `/payments/verify`

---

# Review APIs

## Get Reviews

GET `/products/:id/reviews`

---

## Add Review

POST `/products/:id/reviews`

---

## Update Review

PUT `/reviews/:id`

---

## Delete Review

DELETE `/reviews/:id`

---

# Coupon APIs

## Apply Coupon

POST `/coupons/apply`

---

# Admin APIs

## Dashboard

GET `/admin/dashboard`

---

## Products

GET `/admin/products`

POST `/admin/products`

PUT `/admin/products/:id`

DELETE `/admin/products/:id`

---

## Categories

GET `/admin/categories`

POST `/admin/categories`

PUT `/admin/categories/:id`

DELETE `/admin/categories/:id`

---

## Brands

GET `/admin/brands`

POST `/admin/brands`

PUT `/admin/brands/:id`

DELETE `/admin/brands/:id`

---

## Orders

GET `/admin/orders`

PATCH `/admin/orders/:id`

---

## Users

GET `/admin/users`

PATCH `/admin/users/:id/block`

PATCH `/admin/users/:id/unblock`

---

## Coupons

GET `/admin/coupons`

POST `/admin/coupons`

PUT `/admin/coupons/:id`

DELETE `/admin/coupons/:id`

---

# HTTP Status Codes

| Code | Description |
|------|-------------|
| 200 | Success |
| 201 | Resource Created |
| 204 | No Content |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 409 | Conflict |
| 500 | Internal Server Error |

---

# Authentication

Protected APIs require a valid JWT Access Token.

Authorization header

```
Authorization: Bearer <access_token>
```

---

# API Versioning

Current Version

```
/api/v1
```

Future versions

```
/api/v2
```

---

# Notes

- All responses are JSON.
- All timestamps use ISO 8601 format.
- Soft deletion may be used where appropriate.
- API validation is performed before business logic execution.