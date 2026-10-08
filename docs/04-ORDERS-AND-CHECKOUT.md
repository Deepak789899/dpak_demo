# Orders, Cart & Checkout API Specification
**Target Developer:** Python Backend Engineer

---

## 1. Cart Management Strategy

- **Buyer Frontend Flow:** Cart items are stored optimistically in frontend state and `localStorage` so users can shop without mandatory initial login.
- **Cart Sync on Checkout:** When checking out, the frontend posts the cart items to create the order.
- *(Optional backend cart endpoint)*: `GET /api/v1/cart` and `PUT /api/v1/cart` if persistent multi-device cart sync is desired.

---

## 2. Order Lifecycle States

An order transitions through the following statuses:

```
[ PENDING ] 
     │
     ▼
[ PROCESSING ]  ──► (Payment confirmed, packing items)
     │
     ▼
[ SHIPPED ]     ──► (Tracking number assigned by Admin)
     │
     ▼
[ DELIVERED ]   ──► (Customer received package)
     │
   (or)
[ CANCELLED ]   ──► (Order revoked / refunded)
```

---

## 3. Endpoints

### 3.1. Create Order (Checkout)
`POST /api/v1/orders`  
**Headers Required:** `Authorization: Bearer <customer_token>`

#### Request Body:
```json
{
  "items": [
    {
      "productId": "prod_101",
      "quantity": 2,
      "priceAtPurchase": 289.99
    },
    {
      "productId": "prod_104",
      "quantity": 1,
      "priceAtPurchase": 149.00
    }
  ],
  "shippingAddress": {
    "fullName": "Jane Doe",
    "phone": "+1 555 019 2831",
    "street": "742 Evergreen Terrace",
    "city": "Springfield",
    "state": "OR",
    "postalCode": "97477",
    "country": "USA"
  },
  "paymentMethod": "credit_card",
  "couponCode": "SUMMER10",
  "subtotal": 728.98,
  "discount": 72.90,
  "tax": 52.48,
  "shippingFee": 0.00,
  "totalAmount": 708.56
}
```

#### Success Response (`201 Created`):
```json
{
  "success": true,
  "data": {
    "orderId": "ord_990142",
    "orderNumber": "#AG-990142",
    "status": "PROCESSING",
    "totalAmount": 708.56,
    "createdAt": "2026-10-08T12:00:00Z",
    "estimatedDelivery": "2026-10-14"
  },
  "message": "Order placed successfully"
}
```

---

### 3.2. Get User Orders
`GET /api/v1/orders/my-orders`  
**Headers Required:** `Authorization: Bearer <customer_token>`

#### Success Response (`200 OK`):
```json
{
  "success": true,
  "data": [
    {
      "orderId": "ord_990142",
      "orderNumber": "#AG-990142",
      "createdAt": "2026-10-08T12:00:00Z",
      "status": "PROCESSING",
      "totalAmount": 708.56,
      "itemCount": 3,
      "trackingNumber": null
    }
  ]
}
```

---

### 3.3. Admin: List All Orders
`GET /api/v1/admin/orders`  
**Headers Required:** `Authorization: Bearer <admin_token>`

#### Query Parameters:
- `status`: `PENDING` | `PROCESSING` | `SHIPPED` | `DELIVERED` | `CANCELLED`
- `search`: Order ID, customer name, or email
- `page`: int
- `limit`: int

---

### 3.4. Admin: Update Order Status & Shipment Tracking
`PATCH /api/v1/admin/orders/{orderId}/status`  
**Headers Required:** `Authorization: Bearer <admin_token>`

#### Request Body:
```json
{
  "status": "SHIPPED",
  "trackingNumber": "FEDEX-940010002931448",
  "carrier": "FedEx Express",
  "adminNotes": "Dispatched from Seattle fulfillment center."
}
```

#### Success Response (`200 OK`):
```json
{
  "success": true,
  "data": {
    "orderId": "ord_990142",
    "status": "SHIPPED",
    "trackingNumber": "FEDEX-940010002931448",
    "carrier": "FedEx Express",
    "updatedAt": "2026-10-09T08:15:00Z"
  },
  "message": "Order status updated"
}
```
