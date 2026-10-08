# Admin Analytics & Dashboard API Specification
**Target Developer:** Python Backend Engineer

---

## 1. Executive Dashboard Overview
The Admin portal provides real-time business health indicators:
- **Total Gross Revenue** (with % comparison vs last month)
- **Total Orders Count** (and active processing orders)
- **Active Customers Count**
- **Low Stock Inventory Alert** (count of items with stock < 5)
- **Revenue Trend Data** (for weekly / monthly chart)
- **Recent Transactions Feed**

---

## 2. Endpoints

### 2.1. Admin Metrics Summary
`GET /api/v1/admin/analytics/summary`  
**Headers Required:** `Authorization: Bearer <admin_token>`

#### Success Response (`200 OK`):
```json
{
  "success": true,
  "data": {
    "stats": {
      "totalRevenue": 48290.50,
      "revenueChangePct": 14.2,
      "totalOrders": 1284,
      "ordersChangePct": 8.5,
      "totalCustomers": 942,
      "customersChangePct": 19.1,
      "lowStockItems": 4
    },
    "salesTrends": [
      { "period": "Mon", "revenue": 4200, "orders": 35 },
      { "period": "Tue", "revenue": 6100, "orders": 48 },
      { "period": "Wed", "revenue": 5800, "orders": 44 },
      { "period": "Thu", "revenue": 7900, "orders": 62 },
      { "period": "Fri", "revenue": 9400, "orders": 79 },
      { "period": "Sat", "revenue": 8300, "orders": 71 },
      { "period": "Sun", "revenue": 6590, "orders": 55 }
    ],
    "topCategories": [
      { "category": "Audio", "sharePct": 38 },
      { "category": "Wearables", "sharePct": 27 },
      { "category": "Computing", "sharePct": 21 },
      { "category": "Accessories", "sharePct": 14 }
    ]
  }
}
```
