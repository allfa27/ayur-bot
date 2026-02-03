# AYUR BOT Setup Guide

Complete step-by-step guide to set up and run the AYUR BOT multi-vendor herbal marketplace.

## Prerequisites

- Node.js v14 or higher
- MongoDB (local or MongoDB Atlas cloud)
- npm or yarn package manager
- Git (optional)

## Quick Start (5 minutes)

### 1. Frontend Setup

```bash
# Install dependencies
npm install

# Create environment file
echo 'NEXT_PUBLIC_API_URL=http://localhost:5000' > .env.local

# Start development server
npm run dev
```

Frontend runs at `http://localhost:3000`

### 2. Backend Setup

```bash
# Navigate to backend
cd backend

# Install dependencies
npm install

# Create environment file
cp .env.example .env

# Start backend server
npm run dev
```

Backend runs at `http://localhost:5000`

### 3. Database Setup

#### Option A: Local MongoDB

```bash
# Install MongoDB Community Edition
# macOS: brew install mongodb-community
# Windows: Download from mongodb.com/try/download/community
# Linux: sudo apt-get install mongodb

# Start MongoDB
mongod

# MongoDB runs on localhost:27017
```

#### Option B: MongoDB Atlas (Cloud)

1. Go to [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)
2. Create a free account
3. Create a cluster
4. Get connection string
5. Add to `.env`:
```
MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/herbal_marketplace
```

## Detailed Setup

### Frontend Configuration

1. **Install Dependencies**
```bash
npm install
```

2. **Create `.env.local`**
```bash
NEXT_PUBLIC_API_URL=http://localhost:5000
```

3. **Project Structure**
- `app/` - Next.js pages and routes
- `components/` - React components
- `lib/` - Utility functions
- `public/` - Static assets
- `styles/` - Global styles

4. **Install Additional Packages** (if needed)
```bash
npm install swr axios react-hot-toast
```

5. **Run Development Server**
```bash
npm run dev
```

### Backend Configuration

1. **Install Dependencies**
```bash
cd backend
npm install
```

2. **Create `.env` File**
```bash
cp .env.example .env
```

3. **Edit `.env`**
```env
PORT=5000
MONGODB_URI=mongodb://localhost:27017/herbal_marketplace
NODE_ENV=development
JWT_SECRET=your_super_secret_jwt_key_min_32_chars
JWT_EXPIRE=7d
COOKIE_EXPIRE=7
CLIENT_URL=http://localhost:3000
```

4. **Database Setup**
```bash
# MongoDB collections are created automatically
# First request will create schema
```

5. **Run Backend Server**
```bash
npm run dev
```

## Database Schema Setup

The backend automatically creates MongoDB collections on first request. You can manually initialize with:

```bash
# Optional: Create indexes for better performance
cd backend
node -e "
const mongoose = require('mongoose');
require('dotenv').config();
const User = require('./models/User');
const Product = require('./models/Product');
const Order = require('./models/Order');

mongoose.connect(process.env.MONGODB_URI).then(() => {
  console.log('Indexes created successfully');
  process.exit(0);
}).catch(err => {
  console.error('Error:', err);
  process.exit(1);
});
"
```

## Test the Application

### 1. Access URLs

- Frontend: http://localhost:3000
- Backend API: http://localhost:5000
- API Health Check: http://localhost:5000/api/health

### 2. Test User Accounts

**Create new accounts:**
- Go to http://localhost:3000/register
- Sign up as a customer

**Vendor Registration:**
- After login, go to Vendor Register page
- Fill in shop details
- Submit for approval

**Admin Access:**
- Use backend environment variable `ADMIN_EMAIL` and `ADMIN_PASSWORD`
- Email: admin@herbaldoc.com
- Password: admin123456

### 3. Test API Endpoints

```bash
# Health Check
curl http://localhost:5000/api/health

# Register User
curl -X POST http://localhost:5000/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{"name":"John","email":"john@test.com","phone":"9876543210","password":"password123"}'

# Login
curl -X POST http://localhost:5000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"john@test.com","password":"password123"}'

# Get Products
curl http://localhost:5000/api/products
```

## File Structure Details

### Frontend Pages

```
/app
  /page.tsx                    # Homepage
  /login/page.tsx             # User login
  /register/page.tsx          # User registration
  /products/page.tsx          # Products listing
  /cart/page.tsx              # Shopping cart
  /checkout/page.tsx          # Checkout process
  /dashboard/page.tsx         # User dashboard
  /health-education/page.tsx  # Blog/health tips
  /vendor
    /dashboard/page.tsx       # Vendor dashboard
  /admin
    /dashboard/page.tsx       # Admin dashboard
  /layout.tsx                 # Root layout
  /globals.css                # Global styles
```

### Backend Structure

```
/backend
  /models
    - User.js
    - Vendor.js
    - Product.js
    - Category.js
    - Order.js
    - Cart.js
    - Review.js
    - Blog.js
  /routes
    - auth.routes.js
    - user.routes.js
    - vendor.routes.js
    - product.routes.js
    - category.routes.js
    - cart.routes.js
    - order.routes.js
    - review.routes.js
    - blog.routes.js
    - admin.routes.js
  /middleware
    - auth.js
  /utils
    - jwt.js
    - validators.js
    - response.js
  /server.js
  /package.json
  /.env.example
```

## Common Issues & Solutions

### Port Already in Use

```bash
# Find and kill process using port
# macOS/Linux
lsof -i :3000
lsof -i :5000
kill -9 <PID>

# Windows
netstat -ano | findstr :3000
taskkill /PID <PID> /F
```

### MongoDB Connection Error

```bash
# Check MongoDB is running
# macOS: brew services list | grep mongodb
# Linux: sudo systemctl status mongod

# Verify connection string in .env
MONGODB_URI=mongodb://localhost:27017/herbal_marketplace
```

### CORS Issues

Ensure backend `.env` has correct CLIENT_URL:
```env
CLIENT_URL=http://localhost:3000
```

### JWT Token Issues

- Token expires after 7 days
- Reset `JWT_SECRET` in `.env` if changed
- Clear localStorage if token issues persist

## Environment Variables Reference

### Frontend (.env.local)
```env
NEXT_PUBLIC_API_URL=http://localhost:5000
```

### Backend (.env)
```env
# Server
PORT=5000
NODE_ENV=development

# Database
MONGODB_URI=mongodb://localhost:27017/herbal_marketplace

# JWT
JWT_SECRET=your_jwt_secret_key_min_32_chars
JWT_EXPIRE=7d
COOKIE_EXPIRE=7

# CORS
CLIENT_URL=http://localhost:3000

# Admin (optional)
ADMIN_EMAIL=admin@herbaldoc.com
ADMIN_PASSWORD=admin123456
```

## Running Tests

### Frontend Testing
```bash
npm run test
```

### Backend Testing
```bash
cd backend
npm run test
```

## Production Deployment

### Frontend (Vercel/Netlify)

1. Push code to GitHub
2. Connect to Vercel/Netlify
3. Set environment variables
4. Deploy

### Backend (Heroku/Railway/Render)

1. Create account on hosting platform
2. Set environment variables
3. Connect MongoDB Atlas
4. Deploy

See deployment documentation for each platform.

## Next Steps

1. ✅ Setup complete - start using the application
2. 📚 Read the main [README.md](./README.md)
3. 📖 Review API documentation
4. 🚀 Deploy to production
5. 🔐 Configure security settings
6. 💳 Integrate payment gateway
7. 📧 Setup email notifications

## Support

- Check logs for errors
- Review API response format
- Verify environment variables
- Check MongoDB connection
- Review CORS settings

## Troubleshooting Command

```bash
# Test both frontend and backend
echo "Frontend Status:"
curl http://localhost:3000

echo "\nBackend Status:"
curl http://localhost:5000/api/health

echo "\nMongoDB Connection:"
curl http://localhost:5000/api/auth/login
```

---

**Happy Building! 🌿**
