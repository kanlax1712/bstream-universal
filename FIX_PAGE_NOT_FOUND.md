# Fix: Page Not Found Error on Netlify

## Issue
Getting "page not found" error when loading https://abstream.netlify.app/

## Root Cause
Next.js on Netlify requires proper configuration. The build might be successful but the routing isn't working correctly.

## Solution Applied

### ✅ 1. Created `netlify.toml`
Added proper Netlify configuration file at `web/netlify.toml` with:
- Build command
- Publish directory
- Next.js plugin
- Node version

### ✅ 2. Pushed to GitHub
Configuration file committed and pushed - will trigger new deployment.

---

## Verify Netlify Build Settings

**Go to**: https://app.netlify.com/sites/abstream/configuration/deploys

**Check these settings**:

1. **Base directory**: `web` ✅
2. **Build command**: `npm install && npx prisma generate && npm run build` ✅
3. **Publish directory**: `.next` (NOT `web/.next`) ⚠️ **IMPORTANT**
4. **Node version**: `20` ✅

**The publish directory should be `.next` (relative to base directory), not `web/.next`!**

---

## Next Steps

### Step 1: Check Current Deployment
1. Go to: https://app.netlify.com/sites/abstream/deploys
2. Check if the latest deployment completed successfully
3. Look for any build errors in the logs

### Step 2: Verify Build Settings
1. Go to: https://app.netlify.com/sites/abstream/configuration/deploys
2. Click **"Edit settings"**
3. Verify:
   - **Base directory**: `web`
   - **Publish directory**: `.next` (not `web/.next`)
   - **Build command**: `npm install && npx prisma generate && npm run build`

### Step 3: Trigger New Deployment
After updating settings:
1. Go to: https://app.netlify.com/sites/abstream/deploys
2. Click **"Trigger deploy"** → **"Deploy site"**
3. Wait for build to complete (2-5 minutes)

### Step 4: Test Site
After deployment:
1. Visit: https://abstream.netlify.app/
2. Should see the home page (not 404)
3. Test navigation to other pages

---

## Common Issues & Fixes

### Issue 1: Publish Directory Wrong
**Symptom**: 404 on all pages
**Fix**: Set publish directory to `.next` (not `web/.next`)

### Issue 2: Build Fails
**Symptom**: Deployment shows "Failed"
**Fix**: Check build logs for errors, verify environment variables

### Issue 3: Next.js Plugin Not Installed
**Symptom**: Static pages work but dynamic routes don't
**Fix**: The `netlify.toml` includes the plugin - should auto-install

### Issue 4: Database Connection Error
**Symptom**: Page loads but shows errors
**Fix**: Verify `DATABASE_URL` environment variable is set correctly

---

## Expected Behavior After Fix

✅ Home page loads: https://abstream.netlify.app/
✅ Login page works: https://abstream.netlify.app/login
✅ Registration works: https://abstream.netlify.app/register
✅ Search works: https://abstream.netlify.app/search
✅ Video pages work: https://abstream.netlify.app/video/[id]

---

## Quick Check Commands

If you have Netlify CLI installed locally:

```bash
cd /Users/laxmikanth/Documents/Bstream/web
netlify status
netlify open:admin
```

This will show the current deployment status and open the dashboard.

---

## Summary

✅ **Configuration**: `netlify.toml` created and pushed
✅ **Next Step**: Verify publish directory is `.next` (not `web/.next`)
✅ **Action**: Trigger new deployment after verifying settings

**The site should work after the next deployment!** 🚀

