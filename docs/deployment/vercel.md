---
id: vercel
title: Deploy to Vercel
sidebar_label: Vercel
---

# Deploy to Vercel

Vercel is the recommended platform for deploying OpenForum. Built by the creators of Next.js, it provides optimal performance and developer experience for Next.js applications.

## 🚀 Quick Deploy

### One-Click Deploy

The fastest way to deploy OpenForum to Vercel:

[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https://github.com/mercho40/openforum&env=DATABASE_URL,BETTER_AUTH_SECRET,AUTH_RESEND_KEY&envDescription=Required%20environment%20variables%20for%20OpenForum&project-name=my-openforum&repo-name=my-openforum)

This will:

1. Fork the repository to your GitHub account
2. Create a new Vercel project
3. Prompt you to set required environment variables
4. Deploy your forum automatically

### Manual Deploy

For more control over the deployment process:

## 📋 Prerequisites

- **GitHub Account** - For repository hosting
- **Vercel Account** - [Sign up at vercel.com](https://vercel.com)
- **PostgreSQL Database** - Cloud database instance
- **Resend Account** - For email notifications

## 🔧 Step-by-Step Deployment

### 1. Prepare Your Repository

**Fork the Repository**

1. Go to the [OpenForum repository](https://github.com/mercho40/openforum)
2. Click "Fork" to create your own copy
3. Clone your fork locally (optional, for customization)

**Customize (Optional)**

```bash
# Clone your fork
git clone https://github.com/your-username/openforum.git
cd openforum

# Make customizations
# - Update app/layout.tsx for branding
# - Modify components for custom styling
# - Update README.md with your information

# Commit changes
git add .
git commit -m "Customize for my forum"
git push origin main
```

### 2. Set Up Database

**Option A: Neon Database (Recommended)**

1. **Create account** at [neon.tech](https://neon.tech)
2. **Create project** with these settings:
   - Project name: `openforum-db`
   - Region: Choose closest to your users
   - PostgreSQL version: Latest stable
3. **Copy connection string** from dashboard
4. **Save for environment variables**

**Option B: Supabase Database**

1. **Create account** at [supabase.com](https://supabase.com)
2. **Create new project**:
   - Project name: `openforum`
   - Database password: Generate strong password
   - Region: Choose closest to your users
3. **Get connection string**:
   - Go to Settings → Database
   - Copy "Connection string" under "Connection parameters"
   - Replace `[YOUR-PASSWORD]` with your actual password

### 3. Set Up Email Service

**Configure Resend**

1. **Create account** at [resend.com](https://resend.com)
2. **Add your domain**:
   - Go to Domains in dashboard
   - Add your domain (e.g., `yourdomain.com`)
   - Add DNS records as shown
   - Wait for verification (usually 5-10 minutes)
3. **Generate API key**:
   - Go to API Keys
   - Create new key with "Send access"
   - Copy the key (starts with `re_`)

### 4. Deploy to Vercel

**Connect Repository**

1. **Log in to Vercel** at [vercel.com](https://vercel.com)
2. **Import Project**:
   - Click "Add New..." → "Project"
   - Choose "Import Git Repository"
   - Select your OpenForum fork
   - Click "Import"

**Configure Build Settings**
Vercel should automatically detect these settings:

```
Framework Preset: Next.js
Build Command: pnpm build
Output Directory: .next
Install Command: pnpm install
Development Command: pnpm dev
```

If not detected, set them manually.

**Set Environment Variables**
Add these environment variables in Vercel dashboard:

```env
# Database (required)
DATABASE_URL=postgresql://username:password@host:5432/database

# Authentication (required)
BETTER_AUTH_SECRET=your-64-character-cryptographically-secure-secret
BETTER_AUTH_URL=https://your-deployment-url.vercel.app

# Email (required)
AUTH_RESEND_KEY=re_your_resend_api_key

# Application settings
APP_NAME=Your Forum Name
NEXT_PUBLIC_APP_URL=https://your-deployment-url.vercel.app
```

**Deploy**

1. Click "Deploy" to start the build process
2. Wait for deployment to complete (usually 2-3 minutes)
3. Vercel will provide a deployment URL

### 5. Configure OAuth (Optional)

**Google OAuth**

1. **Update redirect URLs** in Google Cloud Console:
   - Production: `https://your-domain.vercel.app/api/auth/callback/google`
   - Add both your custom domain and Vercel domain
2. **Add environment variables** in Vercel:

   ```env
   GOOGLE_CLIENT_ID=your-google-client-id
   GOOGLE_CLIENT_SECRET=your-google-client-secret
   ```

**GitHub OAuth**

1. **Update callback URL** in GitHub OAuth App:
   - Authorization callback URL: `https://your-domain.vercel.app/api/auth/callback/github`
2. **Add environment variables** in Vercel:

   ```env
   GITHUB_CLIENT_ID=your-github-client-id
   GITHUB_CLIENT_SECRET=your-github-client-secret
   ```

### 6. Run Database Migrations

**Option A: Local Migration (Recommended)**

```bash
# Clone your repository locally
git clone https://github.com/your-username/openforum.git
cd openforum

# Install dependencies
pnpm install

# Create .env.local with production DATABASE_URL
echo "DATABASE_URL=your-production-database-url" > .env.local

# Run migrations
pnpm drizzle-kit migrate

# Verify tables were created
pnpm drizzle-kit studio
```

**Option B: Vercel Build Migration**
Add a build script to automatically run migrations:

```json
// package.json
{
  "scripts": {
    "build": "pnpm drizzle-kit migrate && next build"
  }
}
```

:::warning Migration Caution
Be careful with automatic migrations in production. Test thoroughly in a staging environment first.
:::

## 🌐 Custom Domain Setup

### 1. Configure Domain in Vercel

1. **Go to your project** in Vercel dashboard
2. **Navigate to Settings** → "Domains"
3. **Add your domain**:
   - Enter your domain (e.g., `forum.yourdomain.com`)
   - Click "Add"
4. **Configure DNS**:
   - Type: `CNAME`
   - Name: `forum` (or `@` for root domain)
   - Value: `cname.vercel-dns.com`

### 2. Update Environment Variables

Update these variables with your custom domain:

```env
BETTER_AUTH_URL=https://forum.yourdomain.com
NEXT_PUBLIC_APP_URL=https://forum.yourdomain.com
```

### 3. Update OAuth Redirect URLs

Update redirect URLs in your OAuth providers:

- Google: `https://forum.yourdomain.com/api/auth/callback/google`
- GitHub: `https://forum.yourdomain.com/api/auth/callback/github`

## ⚙️ Advanced Configuration

### Environment Variables for Production

**Required Variables**

```env
# Database
DATABASE_URL=postgresql://user:pass@host:5432/db?sslmode=require

# Authentication
BETTER_AUTH_SECRET=your-64-character-production-secret
BETTER_AUTH_URL=https://your-domain.com

# Email
AUTH_RESEND_KEY=re_live_your_production_key

# Application
APP_NAME=Your Forum Name
NEXT_PUBLIC_APP_URL=https://your-domain.com
NODE_ENV=production
```

**Optional Variables**

```env
# OAuth Providers
GOOGLE_CLIENT_ID=your-google-client-id
GOOGLE_CLIENT_SECRET=your-google-client-secret
GITHUB_CLIENT_ID=your-github-client-id
GITHUB_CLIENT_SECRET=your-github-client-secret

# Analytics (if using)
GOOGLE_ANALYTICS_ID=G-XXXXXXXXXX
VERCEL_ANALYTICS_ID=your-vercel-analytics-id
```

### Vercel Configuration File

Create `vercel.json` for advanced settings:

```json
{
  "framework": "nextjs",
  "buildCommand": "pnpm build",
  "devCommand": "pnpm dev",
  "installCommand": "pnpm install",
  "regions": ["iad1"],
  "env": {
    "NODE_ENV": "production"
  },
  "headers": [
    {
      "source": "/(.*)",
      "headers": [
        {
          "key": "X-Frame-Options",
          "value": "DENY"
        },
        {
          "key": "X-Content-Type-Options",
          "value": "nosniff"
        }
      ]
    }
  ]
}
```

### Performance Optimization

**Image Optimization**
Update `next.config.ts` for image domains:

```typescript
/** @type {import('next').NextConfig} */
const nextConfig = {
  images: {
    remotePatterns: [
      {
        protocol: 'https',
        hostname: '**.githubusercontent.com',
      },
      {
        protocol: 'https',
        hostname: '**.googleusercontent.com',
      },
      {
        protocol: 'https',
        hostname: 'your-cdn-domain.com',
      },
    ],
  },
  experimental: {
    useCache: true,
  },
}

export default nextConfig
```

## 🔍 Monitoring and Analytics

### Vercel Analytics

Enable built-in analytics:

1. **Go to project dashboard** in Vercel
2. **Navigate to Analytics** tab
3. **Enable Vercel Analytics**
4. **Add environment variable**:

   ```env
   VERCEL_ANALYTICS_ID=your-analytics-id
   ```

### Error Monitoring

**Sentry Integration**

```bash
# Install Sentry
pnpm add @sentry/nextjs

# Configure in next.config.ts
```

**Vercel Error Monitoring**
Automatically enabled for all Vercel deployments.

## 🚨 Troubleshooting

### Common Deployment Issues

**Build Failures**

```bash
# Check build logs in Vercel dashboard
# Common issues:
# - Missing environment variables
# - TypeScript errors
# - Dependency issues

# Test build locally
pnpm build
```

**Database Connection Issues**

```bash
# Test database connection
psql "your-database-url" -c "SELECT 1;"

# Check SSL requirements
# Neon/Supabase require sslmode=require
```

**Environment Variable Issues**

```bash
# Verify all required variables are set
# Check for typos in variable names
# Ensure secrets are properly generated
```

**OAuth Redirect Issues**

- Verify redirect URLs match exactly
- Check for HTTP vs HTTPS mismatches
- Ensure domains are correctly configured

### Performance Issues

**Slow Page Loads**

- Check database query performance
- Optimize images and assets
- Use Vercel Analytics to identify bottlenecks

**Database Connection Limits**

- Use connection pooling
- Consider upgrading database plan
- Implement proper connection management

## ✅ Post-Deployment Checklist

### Functionality Verification

- [ ] **Homepage loads** without errors
- [ ] **User registration** works
- [ ] **Email verification** functions
- [ ] **Login/logout** works
- [ ] **OAuth providers** work (if configured)
- [ ] **Admin panel** is accessible
- [ ] **Thread creation** works
- [ ] **Post replies** work
- [ ] **Search functionality** works
- [ ] **Mobile responsiveness** is good

### Security Verification

- [ ] **HTTPS is enforced**
- [ ] **Security headers** are set
- [ ] **Database is secure**
- [ ] **Secrets are properly set**
- [ ] **OAuth is configured correctly**

### Performance Verification

- [ ] **Page load times** are acceptable
- [ ] **Database queries** are optimized
- [ ] **Images load** properly
- [ ] **Mobile performance** is good

## 🔗 Next Steps

After successful deployment:

1. **[Set up your first admin account](../getting-started/first-steps)**
2. **[Configure your forum categories](../administration/category-management)**
3. **[Customize your forum branding](../customization/branding)**
4. **[Set up monitoring and analytics](../guides/monitoring)**

---

**Congratulations!** 🎉 Your OpenForum is now live on Vercel. Share your forum URL and start building your community!
