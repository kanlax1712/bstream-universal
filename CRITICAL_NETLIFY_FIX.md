# 🔴 CRITICAL: Fix Page Not Found Error

## The Problem

The "PAGE NOT FOUND" error is caused by incorrect Netlify build settings. When using `@netlify/plugin-nextjs`, the publish directory should **NOT** be set manually.

## ✅ Solution Applied

1. **Updated `netlify.toml`**: Removed the `publish = ".next"` line (plugin handles it)
2. **Fixed configuration**: Let the Next.js plugin manage the output directory

## ⚠️ CRITICAL: Update Netlify Dashboard Settings

**You MUST update these settings in Netlify Dashboard:**

### Step 1: Go to Build Settings
**Link**: https://app.netlify.com/sites/abstream/configuration/deploys

### Step 2: Click "Edit settings"

### Step 3: Update These Settings:

1. **Base directory**: `web` ✅
2. **Build command**: `npm install && npx prisma generate && npm run build` ✅
3. **Publish directory**: **LEAVE EMPTY** or **DELETE** the value ⚠️ **CRITICAL**
   - The `@netlify/plugin-nextjs` plugin will handle this automatically
   - If you have `.next` or `web/.next` there, **DELETE IT**

4. **Node version**: `20` ✅

### Step 4: Save Settings

### Step 5: Trigger New Deployment
1. Go to: https://app.netlify.com/sites/abstream/deploys
2. Click **"Trigger deploy"** → **"Deploy site"**
3. Wait for build to complete

---

## Why This Happens

When using `@netlify/plugin-nextjs`:
- ✅ The plugin automatically detects the Next.js output
- ✅ It handles routing and server-side rendering
- ❌ Setting `publish = ".next"` manually breaks this

**The plugin needs to control the publish directory itself!**

---

## Verification

After the new deployment:

1. **Check Build Logs**: Look for "Next.js plugin detected" message
2. **Check Site**: Visit https://abstream.netlify.app/
3. **Should See**: Home page (not 404)

---

## If Still Not Working

### Check Build Logs

1. Go to: https://app.netlify.com/sites/abstream/deploys
2. Click on the latest deployment
3. Check for errors in the build logs
4. Look for:
   - ✅ "Next.js plugin detected"
   - ✅ "Build completed successfully"
   - ❌ Any errors about missing files or directories

### Common Issues:

1. **Publish directory still set**: Remove it completely
2. **Base directory wrong**: Should be `web` (not empty)
3. **Build command wrong**: Should include `npx prisma generate`
4. **Environment variables missing**: Check all 4 are set

---

## Summary

✅ **Code**: Fixed `netlify.toml` (removed publish directory)
⚠️ **Action Required**: Update Netlify dashboard - **DELETE publish directory setting**
✅ **Next**: Trigger new deployment

**This should fix the 404 error!** 🚀

