# Quick Deployment Instructions

## 🚀 Deploy to GitHub & Netlify in 5 Steps

### Step 1: Create GitHub Repository

1. Go to https://github.com/new
2. Repository name: `bstream`
3. Choose Public or Private
4. **DO NOT** check any boxes (no README, .gitignore, or license)
5. Click "Create repository"

### Step 2: Push to GitHub

```bash
cd /Users/laxmikanth/Documents/Bstream

# Add remote (replace YOUR_USERNAME)
git remote add origin https://github.com/YOUR_USERNAME/bstream.git

# Push to GitHub
git branch -M main
git push -u origin main
```

### Step 3: Deploy to Netlify

1. Go to https://app.netlify.com
2. Sign up/Login with GitHub
3. Click **"Add new site"** → **"Import an existing project"**
4. Choose **GitHub** and authorize
5. Select **bstream** repository
6. Configure build:
   - **Base directory**: `web`
   - **Build command**: `npm install && npx prisma generate && npm run build`
   - **Publish directory**: `web/.next`
7. Click **"Deploy site"**

### Step 4: Set Environment Variables

In Netlify Dashboard → Site settings → Environment variables:

**Add these**:
```
DATABASE_URL=postgresql://user:pass@host:5432/dbname
NEXTAUTH_URL=https://your-site-name.netlify.app
NEXTAUTH_SECRET=your-secret-key-32-chars-minimum
NODE_ENV=production
```

**Get Database**:
- Use Supabase (free): https://supabase.com
- Or Netlify Postgres addon
- Copy connection string to `DATABASE_URL`

**Generate Secret**:
```bash
openssl rand -base64 32
```

### Step 5: Update Database Schema

```bash
cd web

# Edit prisma/schema.prisma
# Change to:
datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

# Commit and push
git add .
git commit -m "Update to PostgreSQL"
git push
```

Netlify will automatically rebuild!

## ✅ Done!

Your site will be live at: `https://your-site-name.netlify.app`

## 📚 Full Instructions

See [GITHUB_DEPLOYMENT.md](./GITHUB_DEPLOYMENT.md) for detailed instructions.

