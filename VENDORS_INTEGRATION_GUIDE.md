# AYUR BOT - Vendors System Integration Guide

Complete guide to integrate the vendors system with the existing AYUR BOT platform.

## Quick Start

### 1. Backend Setup

#### Database Initialization
```bash
# The Vendor model is already created at /backend/models/Vendor.js
# MongoDB will automatically create the collection on first insert

# Collections created:
- vendors
- vendor_analytics  
- vendor_reviews
- vendor_certifications
```

#### Routes Registration
In `/backend/server.js`, add the vendor routes:

```javascript
const vendorRoutes = require('./routes/vendor.routes')
const adminRoutes = require('./routes/admin.routes')

// Add these lines after other route registrations
app.use('/api/vendors', vendorRoutes)
app.use('/api/admin', adminRoutes)
```

#### Environment Variables
Add to `.env`:
```
# Vendor Configuration
VENDOR_COMMISSION_DEFAULT=15
VENDOR_MIN_PRODUCTS=1
VENDOR_APPROVAL_REQUIRED=true
VENDOR_IMAGE_UPLOAD_SIZE=5242880
```

---

### 2. Frontend Integration

#### Header Navigation
The header already includes the vendors link. Verify it's showing:
```tsx
<Link href="/vendors" className="text-sm font-medium text-foreground hover:text-primary">
  Vendors
</Link>
```

#### Homepage
The homepage now displays featured vendors. Verify by checking `/app/page.tsx`.

#### Vendors Routes
- `/vendors` - Vendor listing page
- `/vendors/[id]` - Individual vendor storefront

---

## Component Integration

### VendorCard Component
Use the reusable VendorCard component throughout the app:

```tsx
import VendorCard from '@/components/VendorCard'

// Full variant (listing page)
<VendorCard
  id={vendor.id}
  shopName={vendor.shopName}
  description={vendor.description}
  category={vendor.category}
  rating={vendor.rating}
  reviews={vendor.reviews}
  productCount={vendor.productCount}
  verified={vendor.verified}
  location={vendor.location}
  email={vendor.email}
  badge={vendor.badge}
  variant="full"
/>

// Compact variant (homepage, sidebar)
<VendorCard
  id={vendor.id}
  shopName={vendor.shopName}
  description={vendor.description}
  category={vendor.category}
  rating={vendor.rating}
  reviews={vendor.reviews}
  productCount={vendor.productCount}
  verified={vendor.verified}
  location={vendor.location}
  email={vendor.email}
  variant="compact"
/>
```

---

## API Integration

### Frontend API Service
Create a vendor service in `/lib/vendor-service.ts`:

```typescript
export class VendorService {
  private baseUrl = process.env.NEXT_PUBLIC_API_URL || 'http://localhost:5000/api'

  async getAllVendors(filters?: {
    search?: string
    category?: string
    sort?: string
    page?: number
    limit?: number
  }) {
    const params = new URLSearchParams()
    if (filters) {
      Object.entries(filters).forEach(([key, value]) => {
        if (value) params.append(key, String(value))
      })
    }
    
    const response = await fetch(`${this.baseUrl}/vendors?${params}`)
    return response.json()
  }

  async getVendorById(id: string) {
    const response = await fetch(`${this.baseUrl}/vendors/${id}`)
    return response.json()
  }

  async getVendorProducts(vendorId: string, page = 1) {
    const response = await fetch(
      `${this.baseUrl}/vendors/${vendorId}/products?page=${page}`
    )
    return response.json()
  }

  async registerAsVendor(data: any, token: string) {
    const response = await fetch(`${this.baseUrl}/vendors/register`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'Authorization': `Bearer ${token}`
      },
      body: JSON.stringify(data)
    })
    return response.json()
  }

  async getVendorDashboard(token: string) {
    const response = await fetch(`${this.baseUrl}/vendors/dashboard`, {
      headers: {
        'Authorization': `Bearer ${token}`
      }
    })
    return response.json()
  }

  async getVendorOrders(token: string, status?: string, page = 1) {
    const params = new URLSearchParams()
    if (status) params.append('status', status)
    params.append('page', String(page))
    
    const response = await fetch(`${this.baseUrl}/vendors/orders?${params}`, {
      headers: {
        'Authorization': `Bearer ${token}`
      }
    })
    return response.json()
  }
}

export const vendorService = new VendorService()
```

### Usage in Components
```tsx
'use client'

import { vendorService } from '@/lib/vendor-service'
import { useEffect, useState } from 'react'

export default function VendorsList() {
  const [vendors, setVendors] = useState([])
  const [loading, setLoading] = useState(true)

  useEffect(() => {
    const fetchVendors = async () => {
      try {
        const response = await vendorService.getAllVendors({
          sort: 'rating',
          limit: 10
        })
        setVendors(response.data)
      } catch (error) {
        console.error('Failed to fetch vendors:', error)
      } finally {
        setLoading(false)
      }
    }

    fetchVendors()
  }, [])

  // Component JSX...
}
```

---

## Features Integration

### 1. Linking Products to Vendors
In the Products page, add a vendor filter:

```tsx
<Link href={`/vendors/${product.vendorId}`} className="hover:text-primary">
  {product.vendorName}
</Link>
```

### 2. Vendor Dashboard Link
In user profile/dashboard, add:

```tsx
{user?.isVendor && (
  <Link href="/vendor/dashboard">
    <Button>Go to Vendor Dashboard</Button>
  </Link>
)}
```

### 3. Product Reviews with Vendor Info
Display vendor info in product reviews:

```tsx
<div className="flex items-center gap-2">
  <span className="font-semibold">{review.vendorName}</span>
  <Badge>{review.verified ? 'Verified Seller' : ''}</Badge>
</div>
```

### 4. Cart/Checkout Vendor Display
Group products by vendor in cart:

```tsx
const vendorGroups = products.reduce((acc, product) => {
  const vendorId = product.vendorId
  if (!acc[vendorId]) {
    acc[vendorId] = {
      vendorName: product.vendorName,
      products: []
    }
  }
  acc[vendorId].products.push(product)
  return acc
}, {})
```

---

## Order Flow Integration

### Vendor-Specific Order Processing

```typescript
// In order.routes.js
app.post('/api/orders', authenticate, async (req, res) => {
  const { items } = req.body
  
  // Group items by vendor
  const vendorOrders = {}
  items.forEach(item => {
    if (!vendorOrders[item.vendorId]) {
      vendorOrders[item.vendorId] = []
    }
    vendorOrders[item.vendorId].push(item)
  })
  
  // Create order for each vendor
  const orders = []
  for (const [vendorId, vendorItems] of Object.entries(vendorOrders)) {
    const order = await Order.create({
      userId: req.user.id,
      vendorId,
      items: vendorItems,
      status: 'pending',
      paymentMethod: req.body.paymentMethod
    })
    orders.push(order)
  }
  
  res.json({ success: true, orders })
})
```

---

## Admin Integration

### Vendor Management Dashboard
Create `/app/admin/vendors/page.tsx`:

```tsx
'use client'

import { useEffect, useState } from 'react'
import { vendorService } from '@/lib/vendor-service'

export default function AdminVendorsPage() {
  const [vendors, setVendors] = useState([])
  const [filter, setFilter] = useState('pending')

  useEffect(() => {
    // Fetch vendors with filter
    const fetchVendors = async () => {
      const response = await fetch(
        `/api/admin/vendors?status=${filter}`,
        {
          headers: {
            'Authorization': `Bearer ${localStorage.getItem('token')}`
          }
        }
      )
      setVendors(await response.json())
    }

    fetchVendors()
  }, [filter])

  return (
    // Admin vendor management UI
    <div>
      {/* Vendor list with approve/reject buttons */}
    </div>
  )
}
```

---

## Analytics Integration

### Vendor Analytics Dashboard
The vendor dashboard at `/vendor/dashboard` includes:

```typescript
// Metrics to track
- Total Products
- Total Orders
- Total Sales
- Revenue Breakdown
- Commission Tracking
- Order Status Distribution
- Top Products
- Customer Reviews
```

---

## Payment Integration with Vendors

### COD (Cash on Delivery) Handling
```typescript
// When order is marked as delivered
const order = await Order.findById(orderId)
const vendor = await Vendor.findById(order.vendorId)

// Calculate vendor payout
const commission = (order.total * vendor.commissionRate) / 100
const vendorPayout = order.total - commission

// Record transaction
await Transaction.create({
  vendorId: vendor._id,
  orderId: order._id,
  amount: vendorPayout,
  commission,
  status: 'pending'
})
```

---

## Notification Integration

### Vendor Notifications
```typescript
// Notify vendor of new order
const notification = await Notification.create({
  vendorId: order.vendorId,
  type: 'new_order',
  title: 'New Order Received',
  message: `Order #${order.orderNumber} received for ₹${order.total}`,
  orderId: order._id,
  read: false
})

// Emit real-time notification
io.to(`vendor_${order.vendorId}`).emit('new_order', notification)
```

---

## Search Integration

### Add Vendors to Search
```typescript
// In search functionality
const searchResults = {
  products: await searchProducts(query),
  vendors: await searchVendors(query),
  blogs: await searchBlogs(query)
}
```

### Search Vendors
```typescript
async function searchVendors(query) {
  return Vendor.find({
    $or: [
      { shopName: { $regex: query, $options: 'i' } },
      { shopDescription: { $regex: query, $options: 'i' } },
      { category: { $regex: query, $options: 'i' } }
    ],
    status: 'approved',
    verified: true
  }).limit(5)
}
```

---

## Email Notifications

### Vendor Email Templates

#### Vendor Approval
```html
<h2>Welcome to AYUR BOT!</h2>
<p>Your vendor account has been approved.</p>
<p>Shop Name: {{ shopName }}</p>
<p>Commission Rate: {{ commissionRate }}%</p>
<a href="{{ vendorDashboardUrl }}">Go to Dashboard</a>
```

#### New Order Notification
```html
<h2>New Order Received!</h2>
<p>Order #{{ orderNumber }}</p>
<p>Total Amount: ₹{{ total }}</p>
<p>Your Commission: ₹{{ commission }}</p>
<a href="{{ orderDetailUrl }}">View Order Details</a>
```

---

## Testing Integration

### Test Scenarios
1. Browse vendors page
2. Filter and sort vendors
3. View individual vendor storefront
4. Click on vendor products
5. Check vendor information accuracy
6. Test responsive design
7. Verify vendor links in products
8. Check vendor badges
9. Test wishlist on vendor page
10. Verify contact information

### API Testing
```bash
# Test vendor listing
curl http://localhost:5000/api/vendors

# Test vendor details
curl http://localhost:5000/api/vendors/1

# Test vendor products
curl http://localhost:5000/api/vendors/1/products

# Test vendor registration (requires auth)
curl -X POST http://localhost:5000/api/vendors/register \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <token>" \
  -d '{"shopName": "Test Shop", ...}'
```

---

## Performance Optimization

### Recommended Enhancements
1. **Caching**
   ```typescript
   // Cache vendor data for 1 hour
   const cacheKey = `vendor_${id}`
   const cached = await redis.get(cacheKey)
   if (cached) return JSON.parse(cached)
   ```

2. **Pagination**
   ```typescript
   // Already implemented in vendor routes
   GET /api/vendors?page=1&limit=12
   ```

3. **Image Optimization**
   ```tsx
   import Image from 'next/image'
   <Image
     src={vendor.shopImage}
     alt={vendor.shopName}
     width={200}
     height={200}
   />
   ```

---

## Rollback Guide

If you need to rollback the vendors feature:

1. Remove vendor routes from server.js
2. Delete `/app/vendors` directory
3. Remove VendorCard import from homepage
4. Revert Header component
5. Drop vendor collections from MongoDB

```bash
# MongoDB commands
use ayur_bot
db.vendors.drop()
db.vendor_analytics.drop()
db.vendor_certifications.drop()
```

---

## Support & Troubleshooting

### Common Issues

**Issue**: Vendors page not loading
- Check if `/app/vendors/page.tsx` exists
- Verify Header component includes vendors link
- Check mock data in the page

**Issue**: API endpoints returning 404
- Ensure vendor routes are registered in server.js
- Check route file paths
- Verify database connection

**Issue**: Images not displaying
- Check image paths in mock data
- Ensure placeholder images exist
- Verify image permissions

### Debug Mode
Add debug logging:
```typescript
console.log('[VENDOR API] Fetching vendors:', query)
console.log('[VENDOR API] Response:', response.data)
console.log('[VENDOR CARD] Rendering vendor:', vendor.shopName)
```

---

## Conclusion

The vendors system is fully integrated into AYUR BOT. All components are in place and ready for use. Follow this guide to ensure proper integration with your existing systems.

For questions or issues, refer to:
- `VENDOR_API_DOCS.md` - Complete API documentation
- `VENDORS_FEATURE.md` - Feature documentation
- Backend route files in `/backend/routes/`
