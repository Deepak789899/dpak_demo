# Products Catalog & Inventory API Specification
**Target Developer:** Python Backend Engineer

---

## 1. Product Data Schema

Each product object should adhere to the following schema:

```json
{
  "id": "prod_101",
  "title": "Aura Lumina Studio Headphones",
  "slug": "aura-lumina-studio-headphones",
  "description": "High-fidelity active noise cancelling wireless headphones with acoustic spatial audio tuning, 40-hour battery life, and ultra-plush memory foam cushions.",
  "price": 289.99,
  "originalPrice": 349.99,
  "discountPercent": 17,
  "rating": 4.9,
  "reviewsCount": 142,
  "category": "Audio",
  "tags": ["featured", "wireless", "best-seller"],
  "stock": 38,
  "isAvailable": true,
  "images": [
    "https://images.unsplash.com/photo-1505740420928-5e560c06d30e?w=800&q=80",
    "https://images.unsplash.com/photo-1484704849700-f032a568e944?w=800&q=80"
  ],
  "specs": {
    "Battery": "40 Hours",
    "Connectivity": "Bluetooth 5.3 & 3.5mm",
    "Weight": "245g",
    "Warranty": "2 Years"
  },
  "createdAt": "2026-09-15T12:00:00Z",
  "updatedAt": "2026-10-01T15:30:00Z"
}
```

---

## 2. Public Buyer Endpoints

### 2.1. List & Search Products
`GET /api/v1/products`

#### Query Parameters:
- `search` (string, optional): Search keyword against `title` and `description`.
- `category` (string, optional): Filter by category (e.g. `Audio`, `Wearables`, `Computing`, `Accessories`).
- `minPrice` (float, optional): e.g. `50`.
- `maxPrice` (float, optional): e.g. `500`.
- `sortBy` (string, optional): `price_asc` | `price_desc` | `rating_desc` | `newest` (default).
- `page` (int, default: 1): Page number.
- `limit` (int, default: 12): Items per page.

#### Success Response (`200 OK`):
```json
{
  "success": true,
  "data": {
    "items": [
      {
        "id": "prod_101",
        "title": "Aura Lumina Studio Headphones",
        "price": 289.99,
        "originalPrice": 349.99,
        "rating": 4.9,
        "reviewsCount": 142,
        "category": "Audio",
        "stock": 38,
        "image": "https://images.unsplash.com/photo-1505740420928-5e560c06d30e?w=800&q=80"
      }
    ],
    "pagination": {
      "currentPage": 1,
      "totalPages": 4,
      "totalItems": 45,
      "limit": 12
    }
  }
}
```

---

### 2.2. Product Details
`GET /api/v1/products/{id_or_slug}`

#### Success Response (`200 OK`):
```json
{
  "success": true,
  "data": {
    "id": "prod_101",
    "title": "Aura Lumina Studio Headphones",
    "slug": "aura-lumina-studio-headphones",
    "description": "...",
    "price": 289.99,
    "originalPrice": 349.99,
    "rating": 4.9,
    "reviewsCount": 142,
    "stock": 38,
    "category": "Audio",
    "images": [...],
    "specs": {...}
  }
}
```

---

## 3. Admin Protected Endpoints
*Note: All endpoints below require `Authorization: Bearer <admin_token>`.*

### 3.1. Create Product
`POST /api/v1/admin/products`

#### Request Body:
```json
{
  "title": "CyberGlass Smart Display 4K",
  "category": "Computing",
  "price": 649.00,
  "originalPrice": 720.00,
  "stock": 15,
  "description": "Ultra-thin 32-inch 4K OLED monitor with seamless color grading.",
  "images": [
    "https://images.unsplash.com/photo-1527443224154-c4a3942d3acf?w=800"
  ],
  "specs": {
    "Resolution": "3840 x 2160",
    "Refresh Rate": "144Hz"
  }
}
```

#### Success Response (`201 Created`):
Returns the newly created product object with generated `id`.

---

### 3.2. Update Product
`PUT /api/v1/admin/products/{id}` or `PATCH /api/v1/admin/products/{id}`

#### Request Body (partial or full):
```json
{
  "price": 619.00,
  "stock": 25
}
```

#### Success Response (`200 OK`):
Returns updated product object.

---

### 3.3. Delete / Archive Product
`DELETE /api/v1/admin/products/{id}`

#### Success Response (`200 OK`):
```json
{
  "success": true,
  "message": "Product successfully deleted or archived"
}
```
