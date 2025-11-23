# Bstream - Video Streaming Platform

A full-featured video streaming platform built with Next.js, Prisma, and NextAuth.js. Similar to YouTube, with features for video upload, playback, subscriptions, comments, playlists, live streaming, and analytics.

**🚀 Ready for deployment to any hosting service!**

## 🚀 Features

- **User Management**: Registration with profile photos, age, gender, and automatic location detection
- **Video Upload**: Upload videos up to 2GB with thumbnails and metadata
- **Video Playback**: HTML5 video player with view tracking
- **Subscriptions**: Subscribe to channels and view subscription feed
- **Comments**: Full commenting system with real-time updates
- **Playlists**: Create and manage video playlists
- **Search**: Search videos and channels
- **Live Streaming**: Camera access for live streaming (UI ready)
- **Analytics**: Dashboard with views, watch time, and engagement metrics

## 🛠️ Tech Stack

- **Frontend**: Next.js 16, React 19, TypeScript, Tailwind CSS
- **Backend**: Next.js API Routes, Server Actions
- **Database**: SQLite (dev) / PostgreSQL (production)
- **ORM**: Prisma
- **Authentication**: NextAuth.js
- **File Storage**: Local filesystem (dev) / Cloud storage (production)

## 📋 Prerequisites

- **Node.js**: Version 20.x (LTS)
- **npm**: Version 9.x or higher
- **Git**: For cloning the repository
- **PostgreSQL**: For production deployment (optional for local dev)

## 🏠 Quick Start - Local Development

### Step 1: Clone Repository

```bash
git clone https://github.com/kanlax1712/bstream-universal.git
cd bstream-universal/web
```

### Step 2: Install Dependencies

```bash
npm install
```

### Step 3: Set Up Database

For local development, SQLite is used by default.

```bash
# Run migrations
npm run db:migrate

# Seed database (optional - adds sample data)
npm run db:seed
```

### Step 4: Set Environment Variables

Create `web/.env.local`:

```bash
DATABASE_URL=file:./dev.db
NEXTAUTH_URL=http://localhost:3000
NEXTAUTH_SECRET=local-development-secret-key
NODE_ENV=development
```

Generate a secret:
```bash
openssl rand -base64 32
```

### Step 5: Start Development Server

```bash
npm run dev
```

Visit: **http://localhost:3000**

### Demo Credentials

After seeding:
- **Email**: `creator@bstream.dev`
- **Password**: `watchmore`

---

## 🌐 Deployment

This project is ready to deploy to **any hosting service**:

- ✅ **Vercel** (Recommended for Next.js)
- ✅ **Railway**
- ✅ **Render**
- ✅ **Fly.io**
- ✅ **DigitalOcean**
- ✅ **Self-hosted/Docker**

### Quick Deployment Guide

See **[DEPLOYMENT_GUIDE.md](./DEPLOYMENT_GUIDE.md)** for complete deployment instructions for all platforms.

### Required Environment Variables

```bash
DATABASE_URL=postgresql://user:password@host:5432/database
NEXTAUTH_URL=https://your-domain.com
NEXTAUTH_SECRET=your-secret-key-min-32-chars
NODE_ENV=production
```

Generate `NEXTAUTH_SECRET`:
```bash
openssl rand -base64 32
```

---

## 📚 Documentation

Complete documentation is available:

- **[LOCAL_SETUP.md](./LOCAL_SETUP.md)** - Complete local development setup guide
- **[DEPLOYMENT_GUIDE.md](./DEPLOYMENT_GUIDE.md)** - Deployment to multiple hosting services
- **[DOCUMENTATION.md](./DOCUMENTATION.md)** - Complete project documentation
- **[SETUP_GUIDE.md](./SETUP_GUIDE.md)** - Detailed setup instructions
- **[FEATURES_DOCUMENTATION.md](./FEATURES_DOCUMENTATION.md)** - Feature guide
- **[TECHNICAL_WORKFLOW.md](./TECHNICAL_WORKFLOW.md)** - Technical architecture

---

## 📁 Project Structure

```
bstream-universal/
├── web/                    # Next.js application
│   ├── src/
│   │   ├── app/           # Next.js App Router pages
│   │   ├── components/    # React components
│   │   ├── lib/           # Utilities (Prisma, Auth)
│   │   └── data/          # Data fetching functions
│   ├── prisma/
│   │   ├── schema.prisma  # Database schema
│   │   └── migrations/   # Database migrations
│   ├── public/            # Static files
│   ├── Dockerfile         # Docker containerization
│   └── package.json
├── LOCAL_SETUP.md         # Local development guide
├── DEPLOYMENT_GUIDE.md    # Multi-platform deployment
└── README.md              # This file
```

---

## 🛠️ Available Scripts

### Development

```bash
npm run dev          # Start development server
```

### Database

```bash
npm run db:migrate   # Run database migrations
npm run db:seed      # Seed database with sample data
npx prisma studio    # Open database GUI
```

### Build

```bash
npm run build        # Build for production
npm start            # Start production server
```

### Docker

```bash
docker build -t bstream .
docker run -p 3000:3000 \
  -e DATABASE_URL="postgresql://..." \
  -e NEXTAUTH_URL="http://localhost:3000" \
  -e NEXTAUTH_SECRET="your-secret" \
  bstream
```

---

## 🔒 Security

- Password hashing with bcrypt
- JWT session management
- Input validation with Zod
- SQL injection prevention (Prisma)
- XSS protection
- File upload validation

---

## 🐛 Troubleshooting

### Port Already in Use

```bash
# Kill process on port 3000
lsof -ti:3000 | xargs kill -9
```

### Database Issues

```bash
# Regenerate Prisma client
npx prisma generate

# Reset database (development only)
npx prisma migrate reset
```

### Build Errors

```bash
# Clear Next.js cache
rm -rf .next
npm run build
```

See **[LOCAL_SETUP.md](./LOCAL_SETUP.md)** for more troubleshooting tips.

---

## 📝 License

This project is open source and available under the MIT License.

---

## 🤝 Contributing

Contributions, issues, and feature requests are welcome!

---

## 🙏 Acknowledgments

Built with:
- [Next.js](https://nextjs.org/)
- [Prisma](https://www.prisma.io/)
- [NextAuth.js](https://next-auth.js.org/)
- [Tailwind CSS](https://tailwindcss.com/)

---

**Made with ❤️ for video streaming**

**Ready to deploy anywhere! 🚀**
