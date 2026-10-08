# 📚 E-Commerce Platform: Frontend-Backend Integration Docs
> **For our Python Backend Collaborator**  
> Welcome! This folder contains all the schemas, endpoint specifications, auth models, and contracts that our React frontend expects from your Python backend (FastAPI, Flask, Django, etc.).

---

## 📂 Documentation Directory Contents

1. [01-ARCHITECTURE-OVERVIEW.md](file:///c:/antigravity_demo-project/docs/01-ARCHITECTURE-OVERVIEW.md)
   - Base URL configuration, CORS setup, standard response envelopes (`success`, `data`, `error`), and HTTP status codes.
2. [02-AUTH-AND-ROLES.md](file:///c:/antigravity_demo-project/docs/02-AUTH-AND-ROLES.md)
   - Dual-login role specifications: **Admin** vs **Customer (User)**.
   - JWT tokens, login/register payloads, session verification `/api/v1/auth/me`.
3. [03-PRODUCTS-API.md](file:///c:/antigravity_demo-project/docs/03-PRODUCTS-API.md)
   - Product catalog listing, search, category filters, pagination, and Admin CRUD (Add, Edit, Stock update, Delete).
4. [04-ORDERS-AND-CHECKOUT.md](file:///c:/antigravity_demo-project/docs/04-ORDERS-AND-CHECKOUT.md)
   - Checkout payload, order lifecycle (`PENDING` → `PROCESSING` → `SHIPPED` → `DELIVERED`), tracking numbers, and customer order history.
5. [05-ADMIN-METRICS.md](file:///c:/antigravity_demo-project/docs/05-ADMIN-METRICS.md)
   - Analytics metrics for the Admin Dashboard (revenue, sales trends, orders, top categories).
6. [06-MOCK-DATA-CONTRACTS.json](file:///c:/antigravity_demo-project/docs/06-MOCK-DATA-CONTRACTS.json)
   - Complete copy-paste JSON seed data for mock database seeding, Pydantic schemas, or SQLite/Postgres fixtures.

---

## ⚡ Quick Backend Checklist for Smooth Integration
- [ ] Enable CORS for `http://localhost:5173` (Frontend Vite dev port).
- [ ] Implement JWT token authentication with role claims (`role: "customer"` or `role: "admin"`).
- [ ] Seed the initial database using the mock data provided in `06-MOCK-DATA-CONTRACTS.json`.
- [ ] Standardize errors so the frontend can display crisp user-facing error toasts.
