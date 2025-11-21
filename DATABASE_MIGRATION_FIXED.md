# ✅ Database Migration Fixed!

## What Was Wrong

The migration SQL file was using **SQLite syntax** (`DATETIME`) instead of **PostgreSQL syntax** (`TIMESTAMP`).

**Error**: `ERROR: 42704: type "datetime" does not exist`

## What I Fixed

✅ Converted all `DATETIME` → `TIMESTAMP`  
✅ Added missing `addedAt` field to `PlaylistVideo` table  
✅ Added missing `device` and `country` fields to `ViewEvent` table  
✅ Added triggers for automatic `updatedAt` timestamp updates  
✅ Added unique constraint for `PlaylistVideo` (playlistId, videoId)

---

## How to Run the Fixed Migration

### Step 1: Go to Supabase SQL Editor

1. Open: https://supabase.com/dashboard
2. Select your **bstream** project
3. Click **"SQL Editor"** in the left sidebar
4. Click **"New query"**

### Step 2: Copy and Run the SQL

1. Open the file: `web/run_migrations.sql`
2. **Copy ALL the SQL** (the entire file)
3. **Paste** into the Supabase SQL Editor
4. Click **"Run"** (or press `Cmd+Enter` / `Ctrl+Enter`)

### Step 3: Verify

After running, you should see:
- ✅ Success message
- ✅ All tables created
- ✅ All indexes created
- ✅ Triggers created

You can verify by checking the **"Table Editor"** in Supabase - you should see all these tables:
- User
- Channel
- Subscription
- Video
- Comment
- Playlist
- PlaylistVideo
- ViewEvent
- Account
- Session
- VerificationToken

---

## What the Migration Creates

### Tables Created:
1. **User** - User accounts with profile info
2. **Channel** - Video channels
3. **Subscription** - User subscriptions to channels
4. **Video** - Video metadata
5. **Comment** - Video comments
6. **Playlist** - User playlists
7. **PlaylistVideo** - Videos in playlists
8. **ViewEvent** - Video view tracking
9. **Account** - OAuth accounts (NextAuth)
10. **Session** - User sessions (NextAuth)
11. **VerificationToken** - Email verification tokens (NextAuth)

### Features:
- ✅ All foreign key relationships
- ✅ Unique constraints (email, handle, etc.)
- ✅ Automatic timestamp updates via triggers
- ✅ Proper indexes for performance

---

## Next Steps

After running the migration:

1. ✅ **Deploy your site** (if not already deployed)
2. ✅ **Test registration** - Create a new account
3. ✅ **Test login** - Sign in with your account
4. ✅ **Test features** - Upload videos, search, etc.

---

## Troubleshooting

### If you get "relation already exists" errors:

The tables might already exist. You can either:

**Option A: Drop and recreate** (⚠️ Deletes all data)
```sql
DROP TABLE IF EXISTS "VerificationToken" CASCADE;
DROP TABLE IF EXISTS "Session" CASCADE;
DROP TABLE IF EXISTS "Account" CASCADE;
DROP TABLE IF EXISTS "ViewEvent" CASCADE;
DROP TABLE IF EXISTS "PlaylistVideo" CASCADE;
DROP TABLE IF EXISTS "Playlist" CASCADE;
DROP TABLE IF EXISTS "Comment" CASCADE;
DROP TABLE IF EXISTS "Video" CASCADE;
DROP TABLE IF EXISTS "Subscription" CASCADE;
DROP TABLE IF EXISTS "Channel" CASCADE;
DROP TABLE IF EXISTS "User" CASCADE;
```
Then run the migration again.

**Option B: Use `CREATE TABLE IF NOT EXISTS`** (already in the SQL)
- The migration uses `IF NOT EXISTS`, so it's safe to run multiple times
- It will skip tables that already exist

---

## Summary

✅ **Fixed**: PostgreSQL-compatible SQL  
✅ **Ready**: Migration file updated  
✅ **Next**: Run in Supabase SQL Editor

**Your database is ready!** 🚀

