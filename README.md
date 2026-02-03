# AYUR BOT - Multi-Vendor Herbal Marketplace

A comprehensive full-stack e-commerce platform for herbal and medical products featuring a clean medical/herbal UI theme with role-based access control, vendor management, and admin controls.

## 🎯 Features

### User Features
- User registration and authentication with JWT
- Secure login with password hashing (bcryptjs)
- User dashboard with order history and favorites
- Search, filter, and browse products by category
- Add to cart and manage shopping cart
- Checkout with COD (Cash on Delivery) payment
- Order tracking and invoice generation
- Product reviews and ratings
- Health education blog section

### Vendor Features
- Vendor registration and approval workflow
- Vendor dashboard with analytics
- Product management (add, edit, delete)
- Inventory management
- Sales analytics and reports
- Order management from vendor perspective
- Commission tracking

### Admin Features
- Comprehensive admin dashboard
- User and vendor management
- Product approval workflow
- Category management
- Order management and tracking
- Commission and revenue reports
- Platform settings and configuration
- Approve/reject vendor applications
- Pending product approvals

### Health Education
- Blog posts and health guides
- Herbal remedies information
- Wellness tips and articles
- Search and filter functionality

## 🏗️ Tech Stack

### Frontend
- **Framework**: Next.js 16 (React 19)
- **Styling**: Tailwind CSS v4 + shadcn/ui
- **State Management**: React hooks + localStorage
- **Forms**: Native HTML forms with validation
- **Icons**: Lucide React

### Backend
- **Runtime**: Node.js
- **Framework**: Express.js
- **Database**: MongoDB
- **Authentication**: JWT (jsonwebtoken)
- **Password Hashing**: bcryptjs
- **CORS**: Enable cross-origin requests

### Database Collections
- Users
- Vendors
- Products
- Categories
- Orders
- Carts
- Reviews
- Blogs

## 📁 Project Structure

```
herbal-marketplace/
├── frontend/
│   ├── app/
│   │   ├── page.tsx                 # Homepage
│   │   ├── login/
│   │   ├── register/
│   │   ├── products/
│   │   ├── dashboard/               # User dashboard
│   │   ├── vendor/
│   │   │   └── dashboard/           # Vendor dashboard
│   │   ├── admin/
│   │   │   └── dashboard/           # Admin dashboard
│   │   ├── layout.tsx
│   │   └── globals.css
│   ├── components/
│   │   ├── Header.tsx
│   │   ├── Footer.tsx
│   │   └── ui/                      # shadcn/ui components
│   └── package.json
│
├── backend/
│   ├── models/
│   │   ├── User.js
│   │   ├── Vendor.js
│   │   ├── Product.js
│   │   ├── Category.js
│   │   ├── Order.js
│   │   ├── Cart.js
│   │   ├── Review.js
│   │   └── Blog.js
│   ├── routes/
│   │   ├── auth.routes.js
│   │   ├── user.routes.js
│   │   ├── vendor.routes.js
│   │   ├── product.routes.js
│   │   ├── category.routes.js
│   │   ├── cart.routes.js
│   │   ├── order.routes.js
│   │   ├── review.routes.js
│   │   ├── blog.routes.js
│   │   └── admin.routes.js
│   ├── middleware/
│   │   └── auth.js
│   ├── utils/
│   │   ├── jwt.js
│   │   ├── validators.js
│   │   └── response.js
│   ├── server.js
│   ├── package.json
│   └── .env.example
```

## 🚀 Getting Started

### Prerequisites
- Node.js (v14+)
- MongoDB (local or Atlas)
- npm or yarn

### Frontend Setup

1. Navigate to the project root
2. Install dependencies:
```bash
npm install
```

3. Create `.env.local`:
```bash
NEXT_PUBLIC_API_URL=http://localhost:5000
```

4. Run development server:
```bash
npm run dev
```

Visit `http://localhost:3000`

### Backend Setup

1. Navigate to backend directory:
```bash
cd backend
```

2. Install dependencies:
```bash
npm install
```

3. Create `.env` file:
```bash
cp .env.example .env
```

4. Update `.env` with your MongoDB URI:
```env
PORT=5000
MONGODB_URI=mongodb://localhost:27017/herbal_marketplace
NODE_ENV=development
JWT_SECRET=your_jwt_secret_key_here
JWT_EXPIRE=7d
```

5. Run the server:
```bash
npm run dev
```

Backend runs on `http://localhost:5000`

## 📡 API Endpoints

### Authentication
- `POST /api/auth/register` - User registration
- `POST /api/auth/login` - User login
- `GET /api/auth/me` - Get current user
- `PUT /api/auth/profile` - Update profile
- `POST /api/auth/logout` - Logout

### Products
- `GET /api/products` - Get all products (with filters)
- `GET /api/products/:id` - Get product details
- `POST /api/products` - Create product (vendor)
- `PUT /api/products/:id` - Update product (vendor)
- `DELETE /api/products/:id` - Delete product (vendor)
- `PUT /api/products/:id/approve` - Approve product (admin)

### Vendors
- `POST /api/vendors/register` - Register as vendor
- `GET /api/vendors` - Get all vendors
- `GET /api/vendors/profile/:id` - Get vendor profile
- `GET /api/vendors/me` - Get my vendor profile
- `PUT /api/vendors/me` - Update vendor profile
- `PUT /api/vendors/:id/status` - Update vendor status (admin)

### Cart
- `GET /api/cart` - Get user cart
- `POST /api/cart/add` - Add item to cart
- `PUT /api/cart/update/:productId` - Update cart item
- `DELETE /api/cart/remove/:productId` - Remove from cart
- `DELETE /api/cart/clear` - Clear cart

### Orders
- `POST /api/orders` - Create order
- `GET /api/orders` - Get user orders
- `GET /api/orders/:id` - Get order details
- `PUT /api/orders/:id/status` - Update order status

### Reviews
- `GET /api/reviews/product/:productId` - Get product reviews
- `POST /api/reviews` - Create review
- `PUT /api/reviews/:id` - Update review
- `DELETE /api/reviews/:id` - Delete review

### Admin
- `GET /api/admin/dashboard/stats` - Dashboard statistics
- `GET /api/admin/reports/revenue` - Revenue report
- `GET /api/admin/approvals/pending` - Pending approvals
- `PUT /api/admin/vendor/:vendorId/approve` - Approve vendor
- `GET /api/admin/settings` - Get platform settings
- `PUT /api/admin/settings` - Update platform settings

## 🔐 Authentication & Authorization

### User Roles
1. **User** - Regular customers
2. **Vendor** - Product sellers
3. **Admin** - Platform administrators

### Protected Routes
- `/dashboard` - User only
- `/vendor/dashboard` - Vendor only
- `/admin/dashboard` - Admin only

### JWT Token
- Tokens are stored in localStorage
- Include token in `Authorization: Bearer <token>` header
- Token expires after 7 days

## 💾 Database Schema

### User
```javascript
{
  name, email, phone, password, role,
  address, profileImage, isActive, isVerified,
  resetPasswordToken, resetPasswordExpire
}
```

### Vendor
```javascript
{
  userId, shopName, shopDescription, category,
  businessRegistration, taxId, bankDetails,
  businessAddress, status, rating, totalOrders,
  totalSales, commissionRate, isActive
}
```

### Product
```javascript
{
  name, slug, description, category, subcategory,
  vendorId, price, discountPrice, quantity, unit,
  images, benefits, ingredients, usage, warnings,
  certification, rating, reviewCount, soldCount, status
}
```

### Order
```javascript
{
  orderNumber, userId, items, shippingAddress,
  subtotal, tax, shippingCost, totalAmount,
  paymentMethod, paymentStatus, orderStatus,
  deliveryDate, invoiceNumber, trackingNumber
}
```

## 💳 Payment & Checkout

### Supported Payment Methods
- **COD** (Cash on Delivery) - Default
- Credit Card (structure ready for future integration)
- UPI (structure ready for future integration)

### Checkout Flow
1. Add items to cart
2. View cart and update quantities
3. Checkout with shipping address
4. Select payment method
5. Order confirmation
6. Order tracking

## 🎨 Design Theme

**Color Palette (Herbal/Medical):**
- **Primary**: Deep Green (#2D5A4F) - Trust & Nature
- **Secondary**: Soft Green (#EBF3F0) - Wellness
- **Accent**: Forest Green (#4A7C6F) - Health
- **Background**: Off-white/Light Blue (#FAFBFC)
- **Text**: Dark Gray (#1A1A1A)

**Typography:**
- Headings: Geist (Sans-serif)
- Body: Geist (Sans-serif)
- Monospace: Geist Mono

## 🧪 Testing

### Sample Test Credentials

**Customer:**
- Email: customer@test.com
- Password: password123

**Vendor:**
- Email: vendor@test.com
- Password: password123

**Admin:**
- Email: admin@herbaldoc.com
- Password: admin123456

## 📝 Environment Variables

### Frontend (.env.local)
```
NEXT_PUBLIC_API_URL=http://localhost:5000
```

### Backend (.env)
```
PORT=5000
MONGODB_URI=mongodb://localhost:27017/herbal_marketplace
NODE_ENV=development
JWT_SECRET=your_jwt_secret_key_here
JWT_EXPIRE=7d
COOKIE_EXPIRE=7
CLIENT_URL=http://localhost:3000
```

## 🚀 Deployment

### Frontend (Vercel)
1. Push to GitHub
2. Connect to Vercel
3. Set environment variables
4. Deploy

### Backend (Heroku/Railway/Render)
1. Create account on hosting platform
2. Connect MongoDB Atlas
3. Set environment variables
4. Deploy

## 🔄 Future Enhancements

- [ ] Payment gateway integration (Stripe, Razorpay)
- [ ] Real-time notifications
- [ ] Advanced analytics dashboard
- [ ] Vendor rating system
- [ ] Product recommendations ML
- [ ] Multi-language support
- [ ] Mobile app (React Native)
- [ ] Subscription plans
- [ ] Loyalty program
- [ ] Seller verification documents
- [ ] Product import/export
- [ ] Advanced search with Elasticsearch

## 📄 License

This project is licensed under the MIT License.

## 🤝 Support

For support, email support@herbaldoc.com or create an issue on the repository.

## 👨‍💻 Author

Created with ❤️ for the herbal and wellness community.

---

**Note**: This is a production-ready template. Update all placeholder values, configure proper security, and test thoroughly before deployment.
