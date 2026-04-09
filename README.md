# 🎭 PoriChat - Anonymous Chat Platform

[![Next.js](https://img.shields.io/badge/Next.js-16-black)](https://nextjs.org/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.0-blue)](https://www.typescriptlang.org/)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind-4.0-38B2AC)](https://tailwindcss.com/)
[![Socket.IO](https://img.shields.io/badge/Socket.IO-4.0-black)](https://socket.io/)
[![Prisma](https://img.shields.io/badge/Prisma-6.0-2D3748)](https://prisma.io/)

PoriChat is a modern, anonymous chat platform that connects strangers instantly. Built with Next.js, Socket.IO, and Prisma.

## ✨ Features

- 🔒 **Anonymous Chat** - Chat without revealing your identity
- 🌍 **Random Matching** - Get paired with random strangers worldwide
- 🛡️ **Secure & Safe** - Report system and moderation
- ⚡ **Real-time Messaging** - Instant message delivery with Socket.IO
- 🎨 **Beautiful UI** - Neomorphic design with Tailwind CSS
- 🌙 **Dark Mode** - Toggle between light and dark themes
- 🌐 **Multi-language** - English & Bengali support
- 👤 **User Accounts** - Optional registration for persistent identity
- 📊 **Admin Dashboard** - Full moderation and analytics tools

## 🚀 Quick Start (Local Development)

### Prerequisites

- Node.js 18+ 
- npm or yarn
- Git

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/voice-call-pro/porichat.git
   cd porichat
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Set up environment variables**
   ```bash
   cp .env.example .env
   # Edit .env with your values
   ```

4. **Set up the database**
   ```bash
   npx prisma generate
   npx prisma db push
   ```

5. **Run the development server**
   ```bash
   npm run dev
   ```

6. **Open [http://localhost:3000](http://localhost:3000)**

## 📁 Project Structure

```
porichat/
├── src/
│   ├── app/              # Next.js App Router
│   │   ├── api/          # API Routes
│   │   ├── page.tsx      # Home Page
│   │   ├── layout.tsx    # Root Layout
│   │   └── globals.css   # Global Styles
│   ├── components/       # React Components
│   │   ├── admin/        # Admin Dashboard
│   │   ├── auth/         # Auth Pages
│   │   ├── chat/         # Chat Components
│   │   ├── layout/       # Layout Components
│   │   └── ui/           # UI Components (shadcn)
│   ├── hooks/            # Custom Hooks
│   ├── lib/              # Utilities
│   ├── store/            # Zustand Store
│   └── middleware.ts     # Next.js Middleware
├── server/
│   └── chat-server.ts    # Socket.IO Server
├── prisma/
│   └── schema.prisma     # Database Schema
├── db/                   # SQLite Database (dev)
├── public/               # Static Assets
└── package.json
```

## 🌐 Full Deployment Guide

### Architecture Overview

```
┌─────────────────┐      ┌─────────────────┐      ┌─────────────────┐
│   Vercel        │──────▶   Render        │──────▶   Render        │
│   (Frontend)    │      │   (API + Chat)  │      │   (PostgreSQL)  │
└─────────────────┘      └─────────────────┘      └─────────────────┘
       https://              https://                  postgresql://
    your-app.vercel.app   your-api.onrender.com
```

---

## 1️⃣ Database Setup (Render PostgreSQL)

### Step 1: Create PostgreSQL Database

1. Go to [Render Dashboard](https://dashboard.render.com/)
2. Click **"New +"** → **"PostgreSQL"**
3. Configure:
   - **Name**: `porichat-db`
   - **Region**: Choose closest to your users
   - **PostgreSQL Version**: 15 or 16
   - **Plan**: Free (or paid for production)
4. Click **"Create Database"**

### Step 2: Get Connection String

After creation, copy the **Internal Database URL**:
```
postgresql://porichat_user:password@your-host.render.com:5432/porichat_db
```

---

## 2️⃣ Backend Deployment (Render)

### Step 1: Create Web Service

1. Go to [Render Dashboard](https://dashboard.render.com/)
2. Click **"New +"** → **"Web Service"**
3. Connect your GitHub repository
4. Configure:

| Setting | Value |
|---------|-------|
| **Name** | `porichat-api` |
| **Region** | Same as database |
| **Branch** | `main` |
| **Runtime** | `Node` |
| **Build Command** | `npm install && npx prisma generate && npm run build` |
| **Start Command** | `npm start` |
| **Plan** | Free (or paid) |

### Step 2: Environment Variables

Add these environment variables in Render Dashboard:

```bash
# Database
DATABASE_URL=postgresql://porichat_user:password@your-host.render.com:5432/porichat_db

# JWT Secret (generate with: openssl rand -base64 32)
JWT_SECRET=your-super-secret-key

# Admin
ADMIN_EMAIL=admin@porichat.com
ADMIN_PASSWORD=your-secure-password

# Frontend URL (will update after Vercel deployment)
NEXT_PUBLIC_APP_URL=https://your-app.vercel.app

# Render sets PORT automatically
```

### Step 3: Deploy

Click **"Create Web Service"**

### Step 4: Deploy Socket.IO Chat Server (Optional)

For better performance, deploy chat server separately:

1. Create another Web Service:
   - **Name**: `porichat-chat`
   - **Build Command**: `npm install && npx tsc -p tsconfig.server.json`
   - **Start Command**: `node dist/server/chat-server.js`

---

## 3️⃣ Frontend Deployment (Vercel)

### Step 1: Import Project

1. Go to [Vercel Dashboard](https://vercel.com/dashboard)
2. Click **"Add New Project"**
3. Import your GitHub repository

### Step 2: Configure Project

| Setting | Value |
|---------|-------|
| **Framework Preset** | `Next.js` |
| **Root Directory** | `./` |
| **Build Command** | `npm run build` |
| **Output Directory** | `dist` |

### Step 3: Environment Variables

Add these in Vercel:

```bash
# API URL (your Render backend URL)
NEXT_PUBLIC_API_URL=https://porichat-api.onrender.com/api

# App URL (will be your Vercel URL)
NEXT_PUBLIC_APP_URL=https://your-app.vercel.app

# Disable server features for static export
NEXT_PUBLIC_STATIC_EXPORT=true
```

### Step 4: Deploy

Click **"Deploy"**

### Step 5: Update CORS

Go back to Render Dashboard and update:
```bash
NEXT_PUBLIC_APP_URL=https://your-app.vercel.app  # Your actual Vercel URL
```

Redeploy the backend.

---

## 🔧 Post-Deployment Setup

### 1. Access Admin Dashboard

1. Go to `https://your-app.vercel.app`
2. Navigate to `/admin`
3. Login with credentials set in environment variables

### 2. Configure Settings

In Admin Dashboard:
- Set chat rules
- Configure ban policies
- Set up report categories

### 3. Test Everything

- [ ] Anonymous chat works
- [ ] User registration works
- [ ] Real-time messaging works
- [ ] Report system works
- [ ] Admin dashboard accessible

---

## 🛠️ Troubleshooting

### CSS Not Loading

**Problem**: Styles not applied after deployment

**Solution**: 
```bash
# Rebuild with proper CSS
rm -rf .next dist
npm run build
```

### Database Connection Error

**Problem**: `Can't reach database server`

**Solution**:
1. Check `DATABASE_URL` format
2. Verify database is running on Render
3. Allow external connections in Render settings

### Socket.IO Not Connecting

**Problem**: Chat not working

**Solution**:
1. Check `SOCKET_URL` environment variable
2. Verify CORS settings include your frontend URL
3. Check Render logs for errors

### Build Fails on Vercel

**Problem**: Build timeout or memory error

**Solution**:
1. Increase build timeout in Vercel settings
2. Use `vercel --prod` for deployment
3. Consider upgrading plan

---

## 📚 API Documentation

### Authentication

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/api/auth/signup` | POST | Register new user |
| `/api/auth/login` | POST | Login user |
| `/api/auth/me` | GET | Get current user |
| `/api/auth/refresh` | POST | Refresh JWT token |

### Chat

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/api/reports` | POST | Report a user |

### Admin

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/api/admin/login` | POST | Admin login |
| `/api/admin/stats` | GET | Get statistics |
| `/api/admin/users` | GET | List users |
| `/api/admin/bans` | GET/POST | Manage bans |
| `/api/admin/reports` | GET | View reports |
| `/api/admin/logs` | GET | System logs |

---

## 🔒 Security Checklist

- [ ] Change default admin password
- [ ] Use strong JWT secret (32+ chars)
- [ ] Enable HTTPS only
- [ ] Set up rate limiting
- [ ] Configure CORS properly
- [ ] Regular database backups
- [ ] Monitor logs for abuse

---

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing`)
3. Commit changes (`git commit -m 'Add feature'`)
4. Push to branch (`git push origin feature/amazing`)
5. Open a Pull Request

---

## 📄 License

MIT License - see [LICENSE](LICENSE) file

---

## 💬 Support

- Email: contact@porichat.com
- Issues: [GitHub Issues](https://github.com/voice-call-pro/porichat/issues)

---

## 🙏 Credits

Built with ❤️ using:
- [Next.js](https://nextjs.org/)
- [Socket.IO](https://socket.io/)
- [Prisma](https://prisma.io/)
- [Tailwind CSS](https://tailwindcss.com/)
- [shadcn/ui](https://ui.shadcn.com/)
