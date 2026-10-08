# Authentication & Role-Based Access Control (RBAC) Specification
**Target Developer:** Python Backend Engineer (FastAPI / Flask / Django REST)

---

## 1. User Roles

The platform enforces two clear roles:
1. **`customer` (Regular User)**:
   - Can register, login, reset password.
   - Can browse products, manage their cart, place orders, and review past orders.
   - Cannot access admin endpoints.
2. **`admin` (System Administrator & Store Manager)**:
   - Has access to the Admin Dashboard.
   - Can create, edit, update inventory, and delete products.
   - Can manage and update order fulfillment statuses (e.g. mark shipped, update tracking numbers).
   - Can view revenue analytics and user lists.

---

## 2. Authentication Flow

- **Token Type:** JSON Web Token (JWT) Bearer Authentication.
- **Header:** `Authorization: Bearer <access_token>`
- **Token Expiry:** Access token (e.g., 60 minutes) + Refresh token (optional, e.g., 7 days).

---

## 3. Endpoints

### 3.1. User Registration
`POST /api/v1/auth/register`

#### Request Body:
```json
{
  "name": "Jane Doe",
  "email": "jane@example.com",
  "password": "SecurePassword123!",
  "role": "customer"
}
```
*Note: Default role should always fall back to `"customer"` if not specified.*

#### Success Response (`201 Created`):
```json
{
  "success": true,
  "data": {
    "user": {
      "id": "usr_98f7e6a1",
      "name": "Jane Doe",
      "email": "jane@example.com",
      "role": "customer",
      "createdAt": "2026-10-08T10:00:00Z"
    },
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6..."
  },
  "message": "Account created successfully"
}
```

---

### 3.2. User / Admin Login
`POST /api/v1/auth/login`

#### Request Body:
```json
{
  "email": "admin@store.com",
  "password": "AdminPassword123!"
}
```

#### Success Response (`200 OK`):
```json
{
  "success": true,
  "data": {
    "user": {
      "id": "adm_11223344",
      "name": "Store Administrator",
      "email": "admin@store.com",
      "role": "admin",
      "avatarUrl": "https://images.unsplash.com/photo-1534528741775-53994a69daeb?w=150"
    },
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6..."
  },
  "message": "Logged in successfully"
}
```

#### Error Response (`401 Unauthorized`):
```json
{
  "success": false,
  "error": {
    "code": "INVALID_CREDENTIALS",
    "message": "Incorrect email or password"
  }
}
```

---

### 3.3. Current User Profile (`/me`)
`GET /api/v1/auth/me`  
**Headers Required:** `Authorization: Bearer <token>`

#### Success Response (`200 OK`):
```json
{
  "success": true,
  "data": {
    "id": "usr_98f7e6a1",
    "name": "Jane Doe",
    "email": "jane@example.com",
    "role": "customer",
    "phone": "+1 555 019 2831",
    "addresses": [
      {
        "id": "addr_1",
        "isDefault": true,
        "street": "742 Evergreen Terrace",
        "city": "Springfield",
        "state": "OR",
        "postalCode": "97477",
        "country": "USA"
      }
    ]
  }
}
```

---

## 4. Protected Route Middleware Guideline for Python

In your FastAPI / Flask / Django framework, protect routes with dependency/decorator:

```python
# FastAPI Example:
from fastapi import Depends, HTTPException, status

async def require_admin(current_user: User = Depends(get_current_user)):
    if current_user.role != "admin":
        raise HTTPException(
            status_code=status.HTTP_403_FORBIDDEN,
            detail="Forbidden: Admin privileges required."
        )
    return current_user
```
