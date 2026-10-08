# Frontend-Backend Integration Guide & API Specifications
**Project:** Modern E-Commerce Platform (React Frontend + Python Backend)  
**Target Audience:** Backend Developer (Python / FastAPI / Flask / Django)  
**Maintained by:** Frontend Lead & UI/UX Designer  

---

## 1. High-Level Architecture Overview

This platform features two distinct roles with tailored experiences:
1. **User (Customer):**
   - Product discovery: search, category filters, price sorting, product details.
   - Shopping: dynamic cart, quantity adjustments, discount codes, checkout flow.
   - Orders: Order history, real-time status tracking (Pending → Processing → Shipped → Delivered).
   - Profile: Shipping address book, account preferences.

2. **Admin (Store Manager & Operations):**
   - Executive Dashboard: revenue stats, orders count, low-stock warnings, sales charts.
   - Product Management: full CRUD (Add, Edit, Stock update, Image URLs, Category tagging, Delete/Archive).
   - Order Management: view all customer orders, filter by status, update shipment tracking number & state.
   - User Overview: list registered customers and their activity.

---

## 2. API Communication Standards

### Base URL
- Local Development: `http://127.0.0.1:8000/api/v1` (or your Python backend port)
- Frontend Vite Proxy or CORS: Ensure `CORS` is enabled for `http://localhost:5173` (Vite default).

### Request Headers
```http
Content-Type: application/json
Accept: application/json
Authorization: Bearer <jwt_access_token>
```

### Standardized Response Envelope
All API endpoints should follow this predictable response structure:

#### Success Response (`200 OK`, `201 Created`):
```json
{
  "success": true,
  "data": { ... },
  "message": "Action completed successfully"
}
```

#### Error Response (`400`, `401`, `403`, `404`, `422`, `500`):
```json
{
  "success": false,
  "error": {
    "code": "RESOURCE_NOT_FOUND",
    "message": "Product with ID 42 does not exist",
    "details": []
  }
}
```

---

## 3. Documentation Map

| Document | Description |
| :--- | :--- |
| **`01-ARCHITECTURE-OVERVIEW.md`** | This file: Base URLs, response format, role breakdown. |
| **`02-AUTH-AND-ROLES.md`** | JWT auth, Login/Register endpoints, role permissions (Admin vs User). |
| **`03-PRODUCTS-API.md`** | Product catalog schemas, filters, pagination, and Admin CRUD endpoints. |
| **`04-ORDERS-AND-CHECKOUT.md`** | Cart schema, Checkout payload, Order lifecycle states, and tracking. |
| **`05-ADMIN-METRICS.md`** | Analytics endpoints for the Admin Dashboard (revenue, counts, charts). |
| **`06-MOCK-DATA-CONTRACTS.json`** | Ready-to-copy JSON sample payloads for your Pydantic / Django models. |
