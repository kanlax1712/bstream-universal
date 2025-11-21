# Database Setup Guide

## ⚠️ Important: DATABASE_URL Format

### Current Value (WRONG for Production)
```
file:./dev.db
```
**This is SQLite format** - only works for local development!

### Required Value (CORRECT for Production)
```
postgresql://user:password@host:5432/dbname
```
**This is PostgreSQL format** - required for Netlify deployment!

---

## Why the Change?

1. **Local Development**: Uses SQLite (`file:./dev.db`)
   - Simple, no setup needed
   - Good for testing
   - Files stored locally

2. **Production (Netlify)**: Requires PostgreSQL
   - SQLite doesn't work on serverless platforms
   - PostgreSQL is cloud-hosted
   - Better for production apps

---

## How to Get PostgreSQL Connection String

### Option 1: Supabase (Recommended - Free & Easy)

1. **Go to**: https://supabase.com
2. **Sign up/Login** (free account)
3. **Click**: "New project"
4. **Fill in**:
   - **Name**: `bstream`
   - **Database password**: (create a strong password - save it!)
   - **Region**: Choose closest to you
5. **Wait**: 2-3 minutes for setup
6. **Go to**: Settings → Database
7. **Scroll to**: "Connection string"
8. **Copy**: The **URI** format (starts with `postgresql://`)
9. **Replace**: `[YOUR-PASSWORD]` with your actual password

**Example format**:
```
postgresql://postgres.xxxxxxxxxxxxx:YOUR_PASSWORD@aws-0-us-east-1.pooler.supabase.com:6543/postgres
```

### Option 2: Neon (Free Tier)

1. **Go to**: https://neon.tech
2. **Sign up** → Create project
3. **Copy** connection string
4. **Format**: Already in correct format

### Option 3: Netlify Postgres Add-on

1. **In Netlify Dashboard**: Add-ons → Postgres
2. **Install** Postgres add-on
3. **Copy** connection string provided

---

## Update DATABASE_URL in Netlify

### Step 1: Get Your PostgreSQL Connection String
Follow one of the options above to get your connection string.

### Step 2: Update in Netlify

**Go to**: https://app.netlify.com/sites/abstream/configuration/env

1. **Find** `DATABASE_URL` variable
2. **Click** to edit
3. **Replace** `file:./dev.db` with your PostgreSQL connection string
4. **Save**

**Example**:
```
Old: file:./dev.db
New: postgresql://postgres.xxxxx:password@host:5432/postgres
```

---

## Complete Environment Variables

After setting up PostgreSQL, your environment variables should be:

| Variable | Value | Example |
|----------|-------|---------|
| `DATABASE_URL` | PostgreSQL connection string | `postgresql://user:pass@host:5432/db` |
| `NEXTAUTH_URL` | Your site URL | `https://abstream.netlify.app` |
| `NEXTAUTH_SECRET` | Secret key | `eye6rx7dN8gUpwsqYx+p/kWli0puo1r+A7q7RuNdTmI=` |
| `NODE_ENV` | Environment | `production` |

---

## After Setting Up Database

### Step 1: Run Migrations

After your first successful build, you need to run database migrations:

**Option A: Via Supabase SQL Editor**
1. Go to your Supabase project
2. Click **"SQL Editor"**
3. Create new query
4. Copy SQL from: `web/prisma/migrations/20251121103358_init/migration.sql`
5. Paste and run
6. Then run: `web/prisma/migrations/20251121143121_add_user_fields/migration.sql`

**Option B: Via Command Line** (if you have local access)
```bash
cd /Users/laxmikanth/Documents/Bstream/web
export DATABASE_URL="your-postgresql-connection-string"
npx prisma migrate deploy
```

### Step 2: Verify

After migrations, your database will have all the tables:
- User
- Channel
- Video
- Comment
- Playlist
- Subscription
- etc.

---

## Summary

**Current Status**:
- ✅ Prisma schema updated to PostgreSQL
- ⚠️ DATABASE_URL still set to SQLite format (temporary)
- ⚠️ Need to set up PostgreSQL database
- ⚠️ Need to update DATABASE_URL with PostgreSQL connection string

**Next Steps**:
1. Set up PostgreSQL database (Supabase recommended)
2. Get connection string
3. Update DATABASE_URL in Netlify
4. Run database migrations
5. Deploy!

---

**The `file:./dev.db` is just a placeholder. You need a real PostgreSQL database for production!**

