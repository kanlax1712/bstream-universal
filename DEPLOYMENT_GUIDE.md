# 🚀 Complete Deployment Guide - Multiple Hosting Services

This guide covers deploying Bstream to various hosting services.

---

## 📋 Prerequisites

### Required Environment Variables

All hosting services need these environment variables:

```bash
DATABASE_URL=postgresql://user:password@host:5432/database
NEXTAUTH_URL=https://your-domain.com
NEXTAUTH_SECRET=your-secret-key-min-32-chars
NODE_ENV=production
```

**Generate NEXTAUTH_SECRET**:
```bash
openssl rand -base64 32
```

---

## 🏠 Local Development Setup

### Step 1: Install Dependencies

```bash
cd web
npm install
```

### Step 2: Set Up Database

**Option A: SQLite (Local Development)**
```bash
# Update web/prisma/schema.prisma
# Change: provider = "sqlite"
# Change: url = env("DATABASE_URL") to url = "file:./dev.db"

npm run db:migrate
npm run db:seed
```

**Option B: PostgreSQL (Production-like)**
```bash
# Use your PostgreSQL connection string
export DATABASE_URL="postgresql://..."
npm run db:migrate
```

### Step 3: Set Environment Variables

Create `web/.env.local`:
```bash
DATABASE_URL=file:./dev.db  # or your PostgreSQL URL
NEXTAUTH_URL=http://localhost:3000
NEXTAUTH_SECRET=your-local-secret-key
NODE_ENV=development
```

### Step 4: Run Development Server

```bash
npm run dev
```

Visit: http://localhost:3000

---

## 🌐 Deployment Options

### Option 1: Vercel (Recommended for Next.js)

**Why Vercel**: Built by Next.js creators, zero-config deployment

#### Steps:

1. **Install Vercel CLI**:
   ```bash
   npm i -g vercel
   ```

2. **Login**:
   ```bash
   vercel login
   ```

3. **Deploy**:
   ```bash
   cd web
   vercel
   ```

4. **Set Environment Variables**:
   - Go to: https://vercel.com/dashboard
   - Select your project
   - Settings → Environment Variables
   - Add all 4 variables

5. **Redeploy**:
   ```bash
   vercel --prod
   ```

**Or use Vercel Dashboard**:
1. Go to: https://vercel.com/new
2. Import your GitHub repository
3. Set root directory: `web`
4. Add environment variables
5. Deploy

---

### Option 2: Railway

**Why Railway**: Easy PostgreSQL setup, simple deployment

#### Steps:

1. **Go to**: https://railway.app
2. **New Project** → **Deploy from GitHub**
3. **Select Repository**: `bstream`
4. **Configure**:
   - Root Directory: `web`
   - Build Command: `npm install && npx prisma generate && npm run build`
   - Start Command: `npm start`
5. **Add PostgreSQL**:
   - Click "+ New" → PostgreSQL
   - Copy connection string
6. **Set Environment Variables**:
   - `DATABASE_URL` = PostgreSQL connection string
   - `NEXTAUTH_URL` = Your Railway domain
   - `NEXTAUTH_SECRET` = Generated secret
   - `NODE_ENV` = `production`
7. **Deploy**: Railway auto-deploys on push

---

### Option 3: Render

**Why Render**: Free tier available, easy setup

#### Steps:

1. **Go to**: https://render.com
2. **New** → **Web Service**
3. **Connect GitHub** repository
4. **Configure**:
   - Name: `bstream`
   - Environment: `Node`
   - Root Directory: `web`
   - Build Command: `npm install && npx prisma generate && npm run build`
   - Start Command: `npm start`
5. **Add PostgreSQL Database**:
   - New → PostgreSQL
   - Copy connection string
6. **Set Environment Variables**:
   - `DATABASE_URL` = PostgreSQL connection string
   - `NEXTAUTH_URL` = Your Render domain
   - `NEXTAUTH_SECRET` = Generated secret
   - `NODE_ENV` = `production`
7. **Deploy**: Render auto-deploys

---

### Option 4: Fly.io

**Why Fly.io**: Global edge deployment, good performance

#### Steps:

1. **Install Fly CLI**:
   ```bash
   curl -L https://fly.io/install.sh | sh
   ```

2. **Login**:
   ```bash
   fly auth login
   ```

3. **Initialize**:
   ```bash
   cd web
   fly launch
   ```

4. **Create `fly.toml`** (if not auto-generated):
   ```toml
   app = "bstream"
   primary_region = "iad"

   [build]
     builder = "paketobuildpacks/builder:base"

   [env]
     NODE_ENV = "production"

   [[services]]
     internal_port = 3000
     protocol = "tcp"
   ```

5. **Set Secrets**:
   ```bash
   fly secrets set DATABASE_URL="postgresql://..."
   fly secrets set NEXTAUTH_URL="https://your-app.fly.dev"
   fly secrets set NEXTAUTH_SECRET="your-secret"
   fly secrets set NODE_ENV="production"
   ```

6. **Deploy**:
   ```bash
   fly deploy
   ```

---

### Option 5: DigitalOcean App Platform

**Why DigitalOcean**: Reliable, good documentation

#### Steps:

1. **Go to**: https://cloud.digitalocean.com/apps
2. **Create App** → **GitHub**
3. **Select Repository**: `bstream`
4. **Configure**:
   - Type: `Web Service`
   - Source Directory: `web`
   - Build Command: `npm install && npx prisma generate && npm run build`
   - Run Command: `npm start`
5. **Add Database**:
   - Resources → Add Database → PostgreSQL
   - Copy connection string
6. **Set Environment Variables**:
   - `DATABASE_URL` = PostgreSQL connection string
   - `NEXTAUTH_URL` = Your DigitalOcean domain
   - `NEXTAUTH_SECRET` = Generated secret
   - `NODE_ENV` = `production`
7. **Deploy**: Auto-deploys on push

---

### Option 6: Self-Hosted (VPS/Docker)

#### Using Docker:

1. **Create `Dockerfile`** in `web/`:
   ```dockerfile
   FROM node:20-alpine AS base

   # Install dependencies
   FROM base AS deps
   WORKDIR /app
   COPY package*.json ./
   RUN npm ci

   # Build
   FROM base AS builder
   WORKDIR /app
   COPY --from=deps /app/node_modules ./node_modules
   COPY . .
   RUN npx prisma generate
   RUN npm run build

   # Production
   FROM base AS runner
   WORKDIR /app
   ENV NODE_ENV production
   COPY --from=builder /app/public ./public
   COPY --from=builder /app/.next/standalone ./
   COPY --from=builder /app/.next/static ./.next/static
   COPY --from=builder /app/node_modules/.prisma ./node_modules/.prisma
   EXPOSE 3000
   CMD ["node", "server.js"]
   ```

2. **Update `next.config.ts`**:
   ```typescript
   const nextConfig: NextConfig = {
     output: 'standalone', // Add this
     // ... rest of config
   };
   ```

3. **Build and Run**:
   ```bash
   docker build -t bstream .
   docker run -p 3000:3000 \
     -e DATABASE_URL="postgresql://..." \
     -e NEXTAUTH_URL="http://localhost:3000" \
     -e NEXTAUTH_SECRET="your-secret" \
     bstream
   ```

---

## 🔧 Database Setup

### For All Hosting Services:

1. **Create PostgreSQL Database**:
   - Use Supabase (free): https://supabase.com
   - Or hosting provider's database service

2. **Run Migrations**:
   ```bash
   # Option A: Via SQL Editor (Supabase)
   # Copy SQL from: web/run_migrations.sql
   # Paste and run in Supabase SQL Editor

   # Option B: Via CLI
   export DATABASE_URL="postgresql://..."
   cd web
   npx prisma migrate deploy
   ```

3. **Verify Tables**:
   - Check that all tables exist
   - Verify connection works

---

## ✅ Post-Deployment Checklist

After deployment:

- [ ] Site loads at your domain
- [ ] Registration page works
- [ ] Can create new account
- [ ] Login works
- [ ] Database connection works
- [ ] Search functionality works
- [ ] Video pages load
- [ ] Comments work
- [ ] Subscriptions work

---

## 🐛 Troubleshooting

### Build Fails

**Check**:
- Environment variables are set
- Build command includes `npx prisma generate`
- Node version is 20

### Database Connection Errors

**Solutions**:
- Verify `DATABASE_URL` is correct
- Check database allows connections
- Ensure migrations ran
- Check firewall/network settings

### Site Loads but Features Don't Work

**Solutions**:
- Check browser console for errors
- Verify environment variables
- Check server logs
- Ensure database migrations ran

---

## 📚 Quick Reference

### Local Development
```bash
cd web
npm install
npm run db:migrate
npm run db:seed
npm run dev
```

### Production Build
```bash
cd web
npm install
npx prisma generate
npm run build
npm start
```

### Environment Variables Template
```bash
DATABASE_URL=postgresql://user:pass@host:5432/db
NEXTAUTH_URL=https://your-domain.com
NEXTAUTH_SECRET=generate-with-openssl-rand-base64-32
NODE_ENV=production
```

---

## 🎯 Recommended Hosting Services

1. **Vercel** - Best for Next.js (zero-config)
2. **Railway** - Easiest PostgreSQL setup
3. **Render** - Good free tier
4. **Fly.io** - Global edge deployment
5. **DigitalOcean** - Reliable and scalable

---

## 📝 Notes

- All hosting services support Next.js
- PostgreSQL is required for production
- SQLite works for local development only
- Environment variables must be set correctly
- Database migrations must run after first deployment

---

**Your Bstream platform is ready to deploy to any hosting service!** 🚀
