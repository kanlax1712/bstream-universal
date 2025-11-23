# 🏠 Local Development Setup Guide

Complete guide to run Bstream on your local machine.

---

## ✅ Prerequisites

- **Node.js**: Version 20.x (LTS)
- **npm**: Version 9.x or higher
- **Git**: For cloning the repository

### Check Versions

```bash
node --version  # Should be v20.x.x
npm --version   # Should be 9.x.x or higher
```

---

## 🚀 Quick Start

### Step 1: Navigate to Project

```bash
cd /Users/laxmikanth/Documents/Bstream/web
```

### Step 2: Install Dependencies

```bash
npm install
```

### Step 3: Set Up Database (SQLite for Local)

The project is configured to use SQLite for local development by default.

**Check `web/prisma/schema.prisma`**:
```prisma
datasource db {
  provider = "sqlite"  // For local development
  url      = "file:./dev.db"
}
```

If it's set to PostgreSQL, change it to SQLite for local development.

### Step 4: Run Database Migrations

```bash
npm run db:migrate
```

This creates the database schema in `web/prisma/dev.db`.

### Step 5: Seed Database (Optional)

```bash
npm run db:seed
```

This adds sample data (users, channels, videos) for testing.

### Step 6: Set Environment Variables

Create `web/.env.local`:
```bash
DATABASE_URL=file:./dev.db
NEXTAUTH_URL=http://localhost:3000
NEXTAUTH_SECRET=local-development-secret-key-change-in-production
NODE_ENV=development
```

**Generate a secret**:
```bash
openssl rand -base64 32
```

### Step 7: Start Development Server

```bash
npm run dev
```

### Step 8: Open Browser

Visit: **http://localhost:3000**

---

## 🔐 Default Login Credentials

After seeding the database:

- **Email**: `creator@bstream.dev`
- **Password**: `watchmore`

---

## 📁 Project Structure

```
Bstream/
├── web/                    # Next.js application
│   ├── src/
│   │   ├── app/            # Next.js App Router pages
│   │   ├── components/     # React components
│   │   ├── lib/            # Utilities (Prisma, Auth)
│   │   └── data/           # Data fetching functions
│   ├── prisma/
│   │   ├── schema.prisma   # Database schema
│   │   └── migrations/     # Database migrations
│   ├── public/             # Static files
│   └── package.json
└── README.md
```

---

## 🛠️ Available Scripts

### Development

```bash
npm run dev          # Start development server (http://localhost:3000)
```

### Database

```bash
npm run db:migrate   # Run database migrations
npm run db:seed      # Seed database with sample data
```

### Build

```bash
npm run build        # Build for production
npm start            # Start production server
```

### Linting

```bash
npm run lint         # Run ESLint
```

---

## 🔄 Switching Between SQLite and PostgreSQL

### For Local Development (SQLite)

**`web/prisma/schema.prisma`**:
```prisma
datasource db {
  provider = "sqlite"
  url      = "file:./dev.db"
}
```

**`web/.env.local`**:
```bash
DATABASE_URL=file:./dev.db
```

### For Production (PostgreSQL)

**`web/prisma/schema.prisma`**:
```prisma
datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}
```

**Environment Variables**:
```bash
DATABASE_URL=postgresql://user:password@host:5432/database
```

**After switching**, regenerate Prisma client:
```bash
npx prisma generate
```

---

## 🐛 Troubleshooting

### Port Already in Use

**Error**: `Port 3000 is already in use`

**Solution**:
```bash
# Kill process on port 3000
lsof -ti:3000 | xargs kill -9

# Or use a different port
PORT=3001 npm run dev
```

### Database Locked

**Error**: `SQLite database is locked`

**Solution**:
```bash
# Close any database connections
# Restart the development server
npm run dev
```

### Prisma Client Not Generated

**Error**: `Cannot find module '@prisma/client'`

**Solution**:
```bash
npx prisma generate
npm install
```

### Module Not Found

**Error**: `Cannot find module '...'`

**Solution**:
```bash
# Reinstall dependencies
rm -rf node_modules package-lock.json
npm install
```

### Build Errors

**Error**: TypeScript or build errors

**Solution**:
```bash
# Clear Next.js cache
rm -rf .next
npm run build
```

---

## 📝 Development Tips

### Hot Reload

- Next.js automatically reloads on file changes
- No need to restart the server manually

### Database Changes

After modifying `prisma/schema.prisma`:
```bash
npx prisma migrate dev --name your_migration_name
npx prisma generate
```

### View Database

Use Prisma Studio:
```bash
npx prisma studio
```

Opens at: http://localhost:5555

### Environment Variables

- Use `.env.local` for local development
- Never commit `.env.local` to Git
- `.env.local` is already in `.gitignore`

---

## ✅ Verification Checklist

After setup, verify:

- [ ] `npm run dev` starts without errors
- [ ] Site loads at http://localhost:3000
- [ ] Can register a new account
- [ ] Can login with seeded account
- [ ] Home page shows videos (if seeded)
- [ ] Search functionality works
- [ ] Video pages load
- [ ] Comments work

---

## 🎯 Next Steps

1. **Explore the App**: Navigate through all pages
2. **Test Features**: Try registration, login, search, etc.
3. **Modify Code**: Make changes and see hot reload
4. **Check Database**: Use Prisma Studio to view data
5. **Deploy**: When ready, follow deployment guide

---

## 📚 Additional Resources

- **Next.js Docs**: https://nextjs.org/docs
- **Prisma Docs**: https://www.prisma.io/docs
- **NextAuth Docs**: https://next-auth.js.org

---

**Your local development environment is ready!** 🎉

