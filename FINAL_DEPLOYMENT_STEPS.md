# Final Deployment Steps for Netlify

## ✅ Code Review Complete

### Fixed Issues:
1. ✅ Next.js config syntax error - Fixed
2. ✅ Prisma config fallback - Added
3. ✅ Build configuration - Verified

### Remaining Actions:

## Step 1: Update Prisma Schema (REQUIRED)

**File**: `web/prisma/schema.prisma`

Change line 9 from:
```prisma
provider = "sqlite"
```

To:
```prisma
provider = "postgresql"
```

**Then commit**:
```bash
cd /Users/laxmikanth/Documents/Bstream
git add web/prisma/schema.prisma
git commit -m "Update to PostgreSQL for production"
git push
```

## Step 2: Set Environment Variables in Netlify

**Go to**: https://app.netlify.com/sites/abstream/configuration/env

**Add these 4 variables**:

1. **DATABASE_URL** = `postgresql://...` (from Supabase/Neon)
2. **NEXTAUTH_URL** = `https://abstream.netlify.app`
3. **NEXTAUTH_SECRET** = `eye6rx7dN8gUpwsqYx+p/kWli0puo1r+A7q7RuNdTmI=`
4. **NODE_ENV** = `production`

## Step 3: Verify Build Settings

**Go to**: https://app.netlify.com/sites/abstream/configuration/deploys

**Ensure**:
- Base directory: `web`
- Build command: `npm install && npx prisma generate && npm run build`
- Publish directory: `web/.next`

## Step 4: Deploy

1. Go to: https://app.netlify.com/sites/abstream/deploys
2. Click "Trigger deploy" → "Deploy site"
3. Monitor build logs
4. Wait for completion

## Step 5: Run Database Migrations

After successful build, run migrations on your PostgreSQL database.

---

**Your site will be live at**: https://abstream.netlify.app

