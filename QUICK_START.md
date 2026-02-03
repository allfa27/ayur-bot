# AYUR BOT - Quick Start Guide

Get up and running in 5 minutes!

## Prerequisites
- Node.js v14+ installed
- MongoDB running (local or Atlas)

## One-Command Setup

### Frontend & Backend Combined

```bash
# Terminal 1: Start Frontend
npm install
echo 'NEXT_PUBLIC_API_URL=http://localhost:5000' > .env.local
npm run dev

# Terminal 2: Start Backend
cd backend
cp .env.example .env
npm install
npm run dev
```

Visit:
- Frontend: http://localhost:3000
- Backend: http://localhost:5000
- API Health: http://localhost:5000/api/health

## Frontend Only (3 steps)

```bash
npm install
echo 'NEXT_PUBLIC_API_URL=http://localhost:5000' > .env.local
npm run dev
```

Open http://localhost:3000

## Backend Only (3 steps)

```bash
cd backend
npm install
cp .env.example .env
npm run dev
```

API runs on http://localhost:5000

## MongoDB Setup

### Local MongoDB
```bash
# macOS
brew install mongodb-community
brew services start mongodb-community

# Linux
sudo apt-get install mongodb
sudo systemctl start mongod

# Windows: Download from mongodb.com
```

### MongoDB Atlas (Cloud)
1. Create account at mongodb.com/atlas
2. Create cluster
3. Get connection string
4. Add to `backend/.env`:
```
MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/herbal_marketplace
```

## Test the App

### Register as Customer
1. Go to http://localhost:3000
2. Click "Register"
3. Fill form and create account
4. Explore products

### Test as Vendor
1. Login
2. Go to "Become a Vendor" (Dashboard)
3. Fill vendor registration
4. Wait for admin approval

### Admin Access
- Email: admin@herbaldoc.com
- Password: admin123456
- Visit: http://localhost:3000/admin/dashboard

## API Testing with cURL

```bash
# Health check
curl http://localhost:5000/api/health

# Get all products
curl http://localhost:5000/api/products

# Create user (register)
curl -X POST http://localhost:5000/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "name":"John Doe",
    "email":"john@example.com",
    "phone":"9876543210",
    "password":"password123",
    "confirmPassword":"password123"
  }'

# Login
curl -X POST http://localhost:5000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email":"john@example.com",
    "password":"password123"
  }'
```

## Project Structure

```
herbal-marketplace/
├── app/                    # Frontend pages
├── components/             # React components
├── backend/               # Node.js API
│   ├── models/           # MongoDB schemas
│   ├── routes/           # API routes
│   └── server.js         # Express server
├── README.md             # Full documentation
└── SETUP.md              # Detailed setup
```

## Common Commands

### Frontend
```bash
npm run dev       # Start dev server
npm run build     # Build production
npm run lint      # Check code quality
npm run test      # Run tests
```

### Backend
```bash
npm run dev       # Start with nodemon
npm start         # Start production
npm run test      # Run tests
```

## Environment Variables

### Frontend (.env.local)
```
NEXT_PUBLIC_API_URL=http://localhost:5000
```

### Backend (.env)
```
PORT=5000
MONGODB_URI=mongodb://localhost:27017/herbal_marketplace
NODE_ENV=development
JWT_SECRET=your_secret_key_here
JWT_EXPIRE=7d
```

## Features Available

✅ User authentication
✅ Browse products
✅ Add to cart
✅ Checkout (COD)
✅ Order tracking
✅ Vendor registration
✅ Admin dashboard
✅ Product reviews
✅ Health blog
✅ Search & filter

## Troubleshooting

### Port Already in Use
```bash
# Kill process
lsof -i :3000    # Find PID
kill -9 <PID>    # Kill it
```

### MongoDB Not Connecting
```bash
# Check if running
mongod --version
# Start service
mongod
```

### API Connection Error
- Check backend is running on 5000
- Verify .env.local has correct API_URL
- Clear browser cache

## Useful Links

- [Main README](./README.md) - Full documentation
- [Setup Guide](./SETUP.md) - Detailed setup
- [Project Summary](./PROJECT_SUMMARY.md) - Complete overview
- [Next.js Docs](https://nextjs.org/docs)
- [Express Docs](https://expressjs.com)
- [MongoDB Docs](https://docs.mongodb.com)

## Next Steps

1. ✅ Start servers
2. 📚 Read README.md
3. 🧪 Test all features
4. 🎨 Customize branding
5. 🚀 Deploy to production

## Video Tutorial

For step-by-step video guide:
1. Start frontend: `npm run dev`
2. Start backend: `cd backend && npm run dev`
3. Open browser: http://localhost:3000
4. Create account and explore!

## Support

Having issues? Check:
- `SETUP.md` for detailed setup
- `README.md` for features
- Terminal logs for errors
- MongoDB connection string
- Environment variables

## Quick Reference

```bash
# Start everything
Terminal 1: npm run dev
Terminal 2: cd backend && npm run dev

# Stop
Ctrl + C

# Logs location
Frontend: http://localhost:3000
Backend: http://localhost:5000/api/health
```

---

**Ready to build something amazing!** 🚀

Start both servers and visit http://localhost:3000
