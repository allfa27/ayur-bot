# AYUR BOT - Vendor API Documentation

Complete API documentation for vendor management, product listing, and storefront features.

## Base URL
```
http://localhost:5000/api
```

## Authentication
All protected endpoints require a valid JWT token in the Authorization header:
```
Authorization: Bearer <token>
```

---

## Vendor Endpoints

### 1. Get All Vendors (Public)
**Endpoint:** `GET /vendors`

**Query Parameters:**
- `search` (string) - Search by vendor name or description
- `category` (string) - Filter by category
- `sort` (string) - Sort by: rating, newest, popular, name
- `page` (number) - Page number (default: 1)
- `limit` (number) - Items per page (default: 12)

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "id": "507f1f77bcf86cd799439011",
      "shopName": "Herbal Wellness Co.",
      "shopDescription": "Premium Ayurvedic products",
      "category": "Ayurvedic",
      "rating": 4.8,
      "reviews": 456,
      "totalSales": 12500,
      "productCount": 125,
      "verified": true,
      "location": "Delhi, India",
      "email": "info@herbalwellness.com",
      "badge": "Top Rated",
      "established": "2018"
    }
  ],
  "pagination": {
    "total": 45,
    "page": 1,
    "pages": 4
  }
}
```

---

### 2. Get Single Vendor Details (Public)
**Endpoint:** `GET /vendors/:id`

**Response:**
```json
{
  "success": true,
  "data": {
    "id": "507f1f77bcf86cd799439011",
    "shopName": "Herbal Wellness Co.",
    "shopDescription": "Premium Ayurvedic products",
    "fullDescription": "We are a leading provider of...",
    "category": "Ayurvedic",
    "shopImage": "https://...",
    "bannerImage": "https://...",
    "rating": 4.8,
    "reviews": 456,
    "verified": true,
    "certifications": ["ISO 9001:2015", "GMP Certified"],
    "returnPolicy": "30 days easy returns",
    "shippingPolicy": "Free shipping on orders above ₹500",
    "contact": {
      "phone": "+91 11 XXXX XXXX",
      "email": "info@herbalwellness.com",
      "website": "www.herbalwellness.com"
    },
    "businessAddress": {
      "street": "123 Main St",
      "city": "Delhi",
      "state": "Delhi",
      "country": "India",
      "zipCode": "110001"
    },
    "bankDetails": {
      "accountHolder": "Company Name",
      "accountNumber": "XXXXXX",
      "bankName": "HDFC Bank",
      "ifscCode": "HDFC0000001"
    }
  }
}
```

---

### 3. Register as Vendor (Protected)
**Endpoint:** `POST /vendors/register`

**Authentication:** Required (User must be logged in)

**Request Body:**
```json
{
  "shopName": "Herbal Wellness Co.",
  "shopDescription": "Premium Ayurvedic products from certified suppliers",
  "category": "Ayurvedic",
  "businessRegistration": "REG123456",
  "taxId": "TAX123456",
  "businessAddress": {
    "street": "123 Main St",
    "city": "Delhi",
    "state": "Delhi",
    "country": "India",
    "zipCode": "110001"
  },
  "bankDetails": {
    "accountHolder": "Company Name",
    "accountNumber": "12345678",
    "bankName": "HDFC Bank",
    "ifscCode": "HDFC0000001"
  }
}
```

**Response:**
```json
{
  "success": true,
  "message": "Vendor registration submitted for approval",
  "data": {
    "id": "507f1f77bcf86cd799439011",
    "status": "pending"
  }
}
```

---

### 4. Get Vendor Dashboard (Protected)
**Endpoint:** `GET /vendors/dashboard`

**Authentication:** Required (Must be a vendor)

**Response:**
```json
{
  "success": true,
  "data": {
    "shopName": "Herbal Wellness Co.",
    "stats": {
      "totalProducts": 125,
      "totalOrders": 1250,
      "totalSales": 2500000,
      "revenue": 1875000,
      "pending": 45,
      "completed": 1200,
      "cancelled": 5
    },
    "recentOrders": [
      {
        "orderId": "ORD123456",
        "buyerName": "John Doe",
        "amount": 2500,
        "date": "2024-01-15",
        "status": "completed"
      }
    ],
    "topProducts": [
      {
        "id": "PROD123",
        "name": "Premium Ashwagandha",
        "sales": 456,
        "revenue": 205200
      }
    ],
    "ratings": {
      "average": 4.8,
      "totalReviews": 456
    }
  }
}
```

---

### 5. Update Vendor Profile (Protected)
**Endpoint:** `PUT /vendors/profile`

**Authentication:** Required (Must be the vendor owner)

**Request Body:**
```json
{
  "shopDescription": "Updated description",
  "shopImage": "https://...",
  "bannerImage": "https://...",
  "businessAddress": {
    "street": "456 New St",
    "city": "Mumbai",
    "state": "Maharashtra",
    "country": "India",
    "zipCode": "400001"
  }
}
```

**Response:**
```json
{
  "success": true,
  "message": "Profile updated successfully",
  "data": {
    "id": "507f1f77bcf86cd799439011",
    "shopName": "Herbal Wellness Co.",
    "updated": true
  }
}
```

---

### 6. Get Vendor Products (Public)
**Endpoint:** `GET /vendors/:id/products`

**Query Parameters:**
- `sort` (string) - Sort by: newest, price, rating, bestseller
- `page` (number) - Page number
- `limit` (number) - Items per page

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "id": "PROD123",
      "name": "Premium Ashwagandha Root",
      "description": "Organic ashwagandha root powder",
      "price": 599,
      "discountPrice": 449,
      "category": "Roots",
      "rating": 4.8,
      "reviews": 234,
      "stock": 450,
      "sold": 1250,
      "image": "https://..."
    }
  ],
  "pagination": {
    "total": 125,
    "page": 1,
    "pages": 13
  }
}
```

---

### 7. Approve/Reject Vendor (Admin Only)
**Endpoint:** `PUT /admin/vendors/:id/status`

**Authentication:** Required (Admin role)

**Request Body:**
```json
{
  "status": "approved",
  "reason": "All documents verified",
  "commission": 15
}
```

**Response:**
```json
{
  "success": true,
  "message": "Vendor status updated",
  "data": {
    "id": "507f1f77bcf86cd799439011",
    "status": "approved"
  }
}
```

---

### 8. Get Vendor Analytics (Protected)
**Endpoint:** `GET /vendors/analytics/dashboard`

**Authentication:** Required (Must be the vendor owner)

**Query Parameters:**
- `period` (string) - daily, weekly, monthly, yearly

**Response:**
```json
{
  "success": true,
  "data": {
    "period": "monthly",
    "dateRange": {
      "start": "2024-01-01",
      "end": "2024-01-31"
    },
    "metrics": {
      "orders": 156,
      "revenue": 487500,
      "totalSales": 650000,
      "commission": 97500,
      "netRevenue": 390000,
      "averageOrderValue": 3124.36,
      "conversionRate": 3.2
    },
    "topCategories": [
      {
        "category": "Ayurvedic",
        "sales": 245,
        "revenue": 312500
      }
    ],
    "dailyRevenue": [
      {
        "date": "2024-01-01",
        "orders": 5,
        "revenue": 15750
      }
    ]
  }
}
```

---

### 9. Get Vendor Orders (Protected)
**Endpoint:** `GET /vendors/orders`

**Authentication:** Required (Must be a vendor)

**Query Parameters:**
- `status` (string) - pending, processing, shipped, delivered, cancelled
- `page` (number) - Page number
- `limit` (number) - Items per page

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "orderId": "ORD123456",
      "buyerName": "John Doe",
      "products": [
        {
          "id": "PROD123",
          "name": "Ashwagandha Root",
          "quantity": 2,
          "price": 449
        }
      ],
      "totalAmount": 898,
      "paymentMethod": "cod",
      "status": "pending",
      "orderDate": "2024-01-15",
      "expectedDelivery": "2024-01-18"
    }
  ],
  "pagination": {
    "total": 1250,
    "page": 1,
    "pages": 126
  }
}
```

---

### 10. Update Order Status (Protected)
**Endpoint:** `PUT /vendors/orders/:orderId/status`

**Authentication:** Required (Vendor owner of the order)

**Request Body:**
```json
{
  "status": "shipped",
  "trackingNumber": "TRK123456789",
  "carrier": "FedEx"
}
```

**Response:**
```json
{
  "success": true,
  "message": "Order status updated",
  "data": {
    "orderId": "ORD123456",
    "status": "shipped",
    "updated": true
  }
}
```

---

### 11. Get Commission Reports (Protected)
**Endpoint:** `GET /vendors/reports/commission`

**Authentication:** Required (Must be a vendor)

**Query Parameters:**
- `startDate` (string) - YYYY-MM-DD
- `endDate` (string) - YYYY-MM-DD

**Response:**
```json
{
  "success": true,
  "data": {
    "period": {
      "start": "2024-01-01",
      "end": "2024-01-31"
    },
    "summary": {
      "totalSales": 650000,
      "commissionRate": 15,
      "commissionAmount": 97500,
      "netRevenue": 552500
    },
    "breakdown": [
      {
        "date": "2024-01-01",
        "orders": 5,
        "sales": 15750,
        "commission": 2362.50
      }
    ]
  }
}
```

---

## Error Responses

### 400 Bad Request
```json
{
  "success": false,
  "message": "Validation error",
  "errors": [
    {
      "field": "shopName",
      "message": "Shop name is required"
    }
  ]
}
```

### 401 Unauthorized
```json
{
  "success": false,
  "message": "Authentication required. Please provide a valid token."
}
```

### 403 Forbidden
```json
{
  "success": false,
  "message": "You do not have permission to access this resource"
}
```

### 404 Not Found
```json
{
  "success": false,
  "message": "Vendor not found"
}
```

### 500 Server Error
```json
{
  "success": false,
  "message": "Internal server error"
}
```

---

## Vendor Categories

Available vendor categories:
- `Herbs`
- `Ayurvedic`
- `Homeopathy`
- `Supplements`
- `Organic`
- `Wellness`

---

## Vendor Status

Possible vendor statuses:
- `pending` - Awaiting admin approval
- `approved` - Active and selling
- `suspended` - Temporarily disabled
- `rejected` - Vendor registration rejected
- `inactive` - Vendor voluntarily inactive

---

## Best Practices

1. **Always validate** vendor inputs before submission
2. **Use pagination** for large vendor lists
3. **Cache vendor profiles** for better performance
4. **Monitor commission rates** to ensure profitability
5. **Track order statuses** in real-time
6. **Validate** all vendor documents during registration
7. **Rate limit** vendor API endpoints to prevent abuse

---

## Example Implementation

### Getting Featured Vendors (Frontend)
```javascript
async function getFeaturedVendors() {
  const response = await fetch('http://localhost:5000/api/vendors?limit=3&sort=rating');
  const data = await response.json();
  return data.data;
}
```

### Registering as Vendor (Frontend)
```javascript
async function registerAsVendor(vendorData) {
  const token = localStorage.getItem('token');
  const response = await fetch('http://localhost:5000/api/vendors/register', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'Authorization': `Bearer ${token}`
    },
    body: JSON.stringify(vendorData)
  });
  return response.json();
}
```

### Getting Vendor Orders (Backend/Frontend)
```javascript
async function getVendorOrders(page = 1) {
  const token = localStorage.getItem('token');
  const response = await fetch(`http://localhost:5000/api/vendors/orders?page=${page}&limit=10`, {
    headers: {
      'Authorization': `Bearer ${token}`
    }
  });
  return response.json();
}
```

---

## Rate Limiting

API rate limits per hour:
- Public endpoints: 1000 requests
- Protected endpoints: 500 requests
- Admin endpoints: 200 requests

Exceeding limits will return `429 Too Many Requests`.

---

## Support

For API support and issues, contact: `support@ayurbot.com`
