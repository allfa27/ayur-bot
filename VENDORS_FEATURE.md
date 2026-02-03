# AYUR BOT - Vendors Feature Documentation

Complete documentation for the vendors marketplace functionality in AYUR BOT.

## Overview

The vendors feature is a comprehensive system that enables sellers to register, manage their storefronts, list products, and handle orders. It includes both frontend interfaces and backend APIs for seamless vendor management.

---

## Features Implemented

### 1. Vendor Listing Page (`/vendors`)
A searchable, filterable directory of all active vendors on AYUR BOT.

**Features:**
- Browse all verified vendors
- Search vendors by name or specialty
- Filter by category (Herbs, Ayurvedic, Homeopathy, Supplements, Organic, Wellness)
- Sort by rating, popularity, newest, or A-Z
- View vendor stats (product count, rating, reviews)
- Contact information display (location, email)
- Verified seller badges
- Top-rated seller indicators
- Responsive grid layout (1-3 columns based on screen size)

**Technologies:**
- React with Next.js
- Tailwind CSS for styling
- Lucide icons for UI elements
- shadcn/ui components (Card, Badge, Button, Select, Input)

---

### 2. Individual Vendor Storefront (`/vendors/:id`)
A detailed vendor profile with products, information, and policies.

**Key Sections:**

#### Hero Section
- Banner display
- Vendor logo area
- Quick stats (rating, total sales, years in business)
- Follow/Share buttons

#### Products Tab
- Grid of featured products
- Product details (price, rating, category)
- Wishlist functionality
- "View All Products" link
- Quick add to cart actions

#### About Tab
- Full vendor description
- Certifications and awards display
- Store statistics
- Seller promises section
- Business tenure

#### Policies Tab
- Return policy
- Shipping policy
- Easy-to-read format

#### Contact Tab
- Physical address
- Phone number
- Email address
- Business hours
- Contact form button

**Features:**
- Tabbed navigation for organized content
- Responsive design
- Wishlist management (add/remove products)
- Product quick view
- Back navigation
- Professional layout

---

### 3. Reusable Vendor Card Component (`/components/VendorCard.tsx`)
A flexible vendor card component with two variants.

**Props:**
```typescript
interface VendorCardProps {
  id: string
  shopName: string
  description: string
  category: string
  rating: number
  reviews: number
  productCount: number
  verified: boolean
  location: string
  email: string
  badge?: string | null
  variant?: 'full' | 'compact'
}
```

**Variants:**
- **Full**: Complete vendor card with all information (used on listing page)
- **Compact**: Minimal card with essential info (used on homepage)

**Customizable Elements:**
- Shop name with link
- Description
- Rating display with star icon
- Review count
- Product count
- Verification badge
- Category badge
- Special badges (Top Rated, Verified, Growing)
- Contact information
- Statistics grid
- Call-to-action button

---

### 4. Featured Vendors on Homepage
Integration of vendor cards into the homepage.

**Features:**
- Display top-rated vendors
- Link to full vendors directory
- Responsive grid layout
- Call-to-action buttons
- Consistent branding

---

### 5. Backend Vendor Routes
Complete REST API for vendor operations.

**Implemented Routes:**

| Method | Endpoint | Description | Auth |
|--------|----------|-------------|------|
| GET | `/vendors` | Get all vendors with filtering | Public |
| GET | `/vendors/:id` | Get single vendor details | Public |
| POST | `/vendors/register` | Register as vendor | Protected |
| GET | `/vendors/dashboard` | Vendor dashboard | Protected |
| PUT | `/vendors/profile` | Update vendor profile | Protected |
| GET | `/vendors/:id/products` | Get vendor products | Public |
| GET | `/vendors/analytics/dashboard` | View analytics | Protected |
| GET | `/vendors/orders` | Get vendor orders | Protected |
| PUT | `/vendors/orders/:id/status` | Update order status | Protected |
| GET | `/vendors/reports/commission` | Commission reports | Protected |
| PUT | `/admin/vendors/:id/status` | Approve/reject vendor | Admin |

---

## Data Models

### Vendor Schema

```javascript
{
  userId: ObjectId (ref: User),
  shopName: String (required, unique),
  shopDescription: String (required),
  category: String (Herbs, Ayurvedic, Homeopathy, Supplements, Organic, Wellness),
  shopImage: String,
  bannerImage: String,
  rating: Number (default: 0),
  totalReviews: Number (default: 0),
  totalSales: Number (default: 0),
  verified: Boolean (default: false),
  status: String (pending, approved, suspended, rejected, inactive),
  businessRegistration: String (required),
  taxId: String (required),
  bankDetails: {
    accountHolder: String,
    accountNumber: String,
    bankName: String,
    ifscCode: String
  },
  businessAddress: {
    street: String,
    city: String,
    state: String,
    country: String,
    zipCode: String
  },
  contactPerson: String,
  phone: String,
  email: String,
  website: String,
  certifications: [String],
  commissionRate: Number (default: 15),
  createdAt: Date,
  updatedAt: Date
}
```

---

## User Interface Components

### Navigation Integration
- Added "Vendors" link to header navigation (both desktop and mobile)
- Responsive menu handling
- Consistent branding throughout

### Vendor Card Variations
- **Full cards** with complete information (on listing page)
- **Compact cards** with essential details (on homepage)
- Hover effects and transitions
- Responsive image handling

### Interactive Elements
- Filter dropdowns
- Search input
- Sort options
- Follow/Share buttons
- Wishlist toggles
- Product quick views
- Tab navigation

---

## API Integration Points

### Frontend API Calls
1. **Get all vendors**
   ```typescript
   GET /api/vendors?category=all&sort=rating&search=query
   ```

2. **Get single vendor**
   ```typescript
   GET /api/vendors/:id
   ```

3. **Get vendor products**
   ```typescript
   GET /api/vendors/:id/products
   ```

4. **Get vendor dashboard** (Protected)
   ```typescript
   GET /api/vendors/dashboard
   ```

### Backend Validation
- Email format validation
- Category enumeration
- Required fields validation
- Tax ID and registration verification
- Bank details validation
- Address validation

---

## Vendor Features & Capabilities

### For Vendors
- **Registration**: Apply to become a vendor
- **Dashboard**: View sales, orders, analytics
- **Product Management**: Add, update, delete products
- **Order Management**: Track and manage orders
- **Analytics**: View revenue, sales trends, performance metrics
- **Commission Tracking**: Monitor commission rates and payouts
- **Profile Management**: Update shop information and policies

### For Customers
- **Browse Vendors**: Search and filter vendors by category, rating
- **Vendor Details**: View complete vendor information and policies
- **Product Listings**: See all products from a vendor
- **Reviews & Ratings**: Read vendor reviews and ratings
- **Contact Vendor**: Get vendor contact information
- **Shop Confidently**: Verified seller badges and certifications

### For Admins
- **Vendor Approval**: Approve or reject vendor registrations
- **Performance Monitoring**: Track vendor metrics and compliance
- **Commission Management**: Set and adjust commission rates
- **Vendor Management**: Suspend or deactivate vendors if needed
- **Reporting**: Generate vendor performance reports

---

## Vendor Storefront Features

### Product Display
- High-quality product images
- Price and discount information
- Rating and review count
- Product category tags
- Stock availability
- Wishlist functionality

### Trust Indicators
- Verified seller badge
- Top-rated indicator
- Years in business
- Total sales count
- Certification display
- Review count

### Information Sections
- Shop description
- Business address
- Contact information
- Return and shipping policies
- Operating hours
- Website link

---

## Responsive Design

### Mobile (< 768px)
- Single column layout for vendor cards
- Stacked filter options
- Collapsible sections
- Touch-friendly buttons
- Mobile-optimized search

### Tablet (768px - 1024px)
- Two column grid
- Horizontal filter layout
- Optimized spacing

### Desktop (> 1024px)
- Three column grid
- Full navigation
- Side-by-side layouts
- Advanced filtering

---

## Future Enhancements

### Planned Features
1. **Vendor Analytics Dashboard**
   - Real-time sales data
   - Customer insights
   - Performance trends

2. **Product Import/Export**
   - Bulk product uploads
   - CSV import functionality
   - Product template downloads

3. **Vendor Messaging**
   - Direct vendor-customer chat
   - Order communications
   - Support tickets

4. **Advanced Filtering**
   - Minimum order quantity
   - Delivery time filters
   - Custom attributes

5. **Vendor Certifications**
   - Badge system for vendors
   - Certification verification
   - Quality scores

6. **Vendor Reviews & Ratings**
   - Detailed vendor feedback
   - Review moderation
   - Response system

---

## File Structure

```
/app
├── /vendors
│   ├── page.tsx (Vendor listing)
│   ├── [id]
│   │   └── page.tsx (Vendor storefront)
│   └── loading.tsx (Optional: Loading state)
│
/components
├── VendorCard.tsx (Reusable vendor card)
├── Header.tsx (Updated with vendors link)
└── Footer.tsx (Updated with vendor info)

/backend
├── /models
│   └── Vendor.js (Vendor data model)
├── /routes
│   └── vendor.routes.js (Vendor endpoints)
└── server.js (API configuration)

/docs
├── VENDOR_API_DOCS.md (Complete API documentation)
└── VENDORS_FEATURE.md (This file)
```

---

## Testing Checklist

- [ ] Vendor listing page loads with mock data
- [ ] Search filters work correctly
- [ ] Category filtering works
- [ ] Sorting options work
- [ ] Vendor cards display all information
- [ ] Individual vendor page loads correctly
- [ ] Tab navigation works
- [ ] Wishlist functionality works
- [ ] Responsive design works on mobile/tablet/desktop
- [ ] Links navigate correctly
- [ ] Badges display appropriately
- [ ] Contact information displays correctly
- [ ] Product grid displays properly
- [ ] Header navigation includes vendors link

---

## Performance Optimization

### Implemented
- Component splitting for better code organization
- Reusable card component to reduce duplication
- Proper state management with useState
- Lazy loading considerations

### Recommendations
- Implement image optimization with Next.js Image component
- Add pagination for vendor listing
- Cache vendor data with SWR
- Implement virtual scrolling for large lists
- Optimize bundle size

---

## Security Considerations

1. **Authentication**: Protected vendor routes require JWT tokens
2. **Authorization**: Vendors can only access their own data
3. **Input Validation**: All vendor data is validated on backend
4. **Rate Limiting**: API endpoints have rate limiting
5. **Data Privacy**: Sensitive vendor information is protected

---

## Support & Maintenance

### Common Issues
1. **Vendor not appearing**: Check verification status
2. **Images not loading**: Verify image URLs
3. **Filter not working**: Check filter parameter values
4. **Performance issues**: Implement caching

### Maintenance Tasks
- Monitor vendor performance metrics
- Update vendor certifications
- Verify vendor documents annually
- Clean up inactive vendors
- Update commission rates

---

## Conclusion

The AYUR BOT vendors feature provides a complete marketplace solution for connecting buyers with trusted sellers. The implementation includes responsive UI, comprehensive APIs, and user-friendly interfaces for all stakeholders.

For detailed API documentation, refer to `VENDOR_API_DOCS.md`.
