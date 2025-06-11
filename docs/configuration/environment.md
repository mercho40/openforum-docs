---
id: environment
title: Environment Variables
sidebar_label: Environment Variables
---

# Environment Variables

OpenForum uses environment variables for configuration, allowing you to customize behavior across different environments (development, staging, production) without changing code.

## 📋 Environment File Setup

### Creating Your Environment File

Create a `.env.local` file in your project root:

```bash
# Copy from example (if available)
cp .env.example .env.local

# Or create new file
touch .env.local
```

:::warning Keep Environment Files Secure
- Never commit `.env.local` to version control
- Use strong, unique secrets for production
- Rotate secrets regularly in production environments
:::

## 🔑 Required Variables

These variables are essential for OpenForum to function properly.

### Database Configuration

```env
# PostgreSQL connection string
DATABASE_URL="postgresql://username:password@host:port/database"

# Examples:
# Local: postgresql://postgres:password@localhost:5432/openforum
# Neon: postgresql://username:password@ep-xxx.us-east-1.aws.neon.tech/openforum
# Supabase: postgresql://postgres:password@db.xxx.supabase.co:5432/postgres
```

### Authentication Secrets

```env
# Better Auth secret key (minimum 32 characters)
BETTER_AUTH_SECRET="your-super-secret-key-here-minimum-32-characters"

# Application URL (update for production)
BETTER_AUTH_URL="http://localhost:3000"
```

:::tip Generating Secure Secrets
Generate a cryptographically secure secret:
```bash
# Generate 32-character base64 string
openssl rand -base64 32

# Generate 64-character hex string  
openssl rand -hex 32

# Or use online generator (ensure it's from a trusted source)
```
:::

### Email Service

```env
# Resend API key for email delivery
AUTH_RESEND_KEY="re_xxxxxxxxxxxxxxxxxxxxx"
```

## ⚙️ Optional Variables

These variables enable additional features but aren't required for basic functionality.

### OAuth Providers

#### Google OAuth
```env
GOOGLE_CLIENT_ID="your-google-client-id.googleusercontent.com"
GOOGLE_CLIENT_SECRET="GOCSPX-your-google-client-secret"
```

#### GitHub OAuth
```env
GITHUB_CLIENT_ID="your-github-client-id"
GITHUB_CLIENT_SECRET="your-github-client-secret"
```

### Application Settings

```env
# Custom application name
APP_NAME="Your Forum Name"

# Public application URL
NEXT_PUBLIC_APP_URL="http://localhost:3000"

# Environment designation
NODE_ENV="development"  # or "production"
```

## 🏗️ Environment-Specific Configurations

### Development Environment

```env
# Development settings
NODE_ENV="development"
BETTER_AUTH_URL="http://localhost:3000"
NEXT_PUBLIC_APP_URL="http://localhost:3000"

# Database (can be local or cloud)
DATABASE_URL="postgresql://postgres:password@localhost:5432/openforum"

# Auth secret (can be simple for dev)
BETTER_AUTH_SECRET="dev-secret-key-minimum-32-characters-long"

# Email (use test keys)
AUTH_RESEND_KEY="re_test_xxxxxxxxxxxxxxxxxxxxx"

# OAuth (localhost redirect URLs)
GOOGLE_CLIENT_ID="your-dev-google-client-id"
GOOGLE_CLIENT_SECRET="your-dev-google-secret"
GITHUB_CLIENT_ID="your-dev-github-client-id" 
GITHUB_CLIENT_SECRET="your-dev-github-secret"
```

### Production Environment

```env
# Production settings
NODE_ENV="production"
BETTER_AUTH_URL="https://yourdomain.com"
NEXT_PUBLIC_APP_URL="https://yourdomain.com"

# Production database
DATABASE_URL="postgresql://user:password@production-host:5432/openforum"

# Strong auth secret
BETTER_AUTH_SECRET="very-strong-production-secret-key-64-characters-minimum"

# Production email with verified domain
AUTH_RESEND_KEY="re_live_xxxxxxxxxxxxxxxxxxxxx"

# OAuth with production redirect URLs
GOOGLE_CLIENT_ID="your-prod-google-client-id"
GOOGLE_CLIENT_SECRET="your-prod-google-secret"
GITHUB_CLIENT_ID="your-prod-github-client-id"
GITHUB_CLIENT_SECRET="your-prod-github-secret"

# Application branding
APP_NAME="Your Production Forum"
```

## 🔧 Service-Specific Setup

### Database Services

#### Neon Database
```env
# Get from Neon dashboard
DATABASE_URL="postgresql://username:password@ep-xxx-xxx.us-east-1.aws.neon.tech/openforum?sslmode=require"
```

#### Supabase Database
```env
# Get from Supabase project settings
DATABASE_URL="postgresql://postgres.xxx:password@aws-0-us-east-1.pooler.supabase.com:5432/postgres"
```

#### Railway Database
```env
# Get from Railway database service
DATABASE_URL="postgresql://postgres:password@containers-us-west-xxx.railway.app:5432/railway"
```

#### Local PostgreSQL
```env
# Standard local setup
DATABASE_URL="postgresql://postgres:password@localhost:5432/openforum"

# With custom user
DATABASE_URL="postgresql://myuser:mypassword@localhost:5432/openforum"

# With custom port
DATABASE_URL="postgresql://postgres:password@localhost:5433/openforum"
```

### Email Services

#### Resend (Recommended)
```env
# Get from Resend dashboard
AUTH_RESEND_KEY="re_xxxxxxxxxxxxxxxxxxxxx"
```

#### SendGrid (Alternative)
```env
# If using SendGrid instead
SENDGRID_API_KEY="SG.xxxxxxxxxxxxxxxxxxxxx"
```

### OAuth Provider Setup

#### Google Cloud Console
1. Create project at [console.cloud.google.com](https://console.cloud.google.com)
2. Enable Google+ API
3. Create OAuth 2.0 credentials
4. Set authorized redirect URIs:
   - Development: `http://localhost:3000/api/auth/callback/google`
   - Production: `https://yourdomain.com/api/auth/callback/google`

```env
GOOGLE_CLIENT_ID="123456789-xxxxxxxxxx.googleusercontent.com"
GOOGLE_CLIENT_SECRET="GOCSPX-xxxxxxxxxxxxxxxx"
```

#### GitHub Developer Settings
1. Go to GitHub Settings > Developer settings > OAuth Apps
2. Create new OAuth App
3. Set authorization callback URL:
   - Development: `http://localhost:3000/api/auth/callback/github`
   - Production: `https://yourdomain.com/api/auth/callback/github`

```env
GITHUB_CLIENT_ID="your-client-id"
GITHUB_CLIENT_SECRET="your-client-secret"
```

## 🌐 Platform-Specific Variables

### Vercel Deployment

When deploying to Vercel, add these through the dashboard or CLI:

```bash
# Set production environment variables
vercel env add BETTER_AUTH_SECRET production
vercel env add DATABASE_URL production
vercel env add AUTH_RESEND_KEY production

# Set for all environments
vercel env add APP_NAME production preview development
```

### Railway Deployment

Railway can automatically provide some variables:

```env
# Railway provides these automatically
RAILWAY_STATIC_URL="https://your-app.railway.app"
DATABASE_URL="postgresql://..." # If using Railway PostgreSQL

# You need to set these
BETTER_AUTH_SECRET="your-secret"
AUTH_RESEND_KEY="your-resend-key"
```

### Docker Deployment

For Docker containers, pass environment variables:

```bash
# Docker run with env vars
docker run -e DATABASE_URL="postgresql://..." \
           -e BETTER_AUTH_SECRET="your-secret" \
           -e AUTH_RESEND_KEY="your-key" \
           your-forum-image

# Or use .env file
docker run --env-file .env.production your-forum-image
```

## 🔍 Environment Variable Validation

OpenForum validates critical environment variables at startup:

### Required Variables Check
- `DATABASE_URL` - Must be valid PostgreSQL connection string
- `BETTER_AUTH_SECRET` - Must be at least 32 characters
- `BETTER_AUTH_URL` - Must be valid URL

### Optional Variables Check
- `AUTH_RESEND_KEY` - Must start with "re_" if provided
- OAuth credentials - Must be valid format if provided

## 🐛 Troubleshooting Environment Issues

### Common Problems

**Database Connection Failed**
```bash
# Check if database URL is correct
echo $DATABASE_URL

# Test connection
psql $DATABASE_URL -c "SELECT 1;"
```

**Authentication Errors**
```bash
# Verify secret length
echo -n "$BETTER_AUTH_SECRET" | wc -c  # Should be 32+
```

**Email Not Working**
```bash
# Check Resend key format
echo $AUTH_RESEND_KEY  # Should start with "re_"
```

**OAuth Redirect Mismatch**
- Verify redirect URLs in OAuth provider settings
- Ensure `BETTER_AUTH_URL` matches your domain
- Check for HTTP vs HTTPS mismatches

### Environment Loading Order

Next.js loads environment variables in this order:
1. `.env.local` (highest priority)
2. `.env.production` or `.env.development`
3. `.env`
4. System environment variables

### Variable Precedence

Variables are loaded with this precedence:
1. System environment variables
2. `.env.local`
3. `.env.production` (production build)
4. `.env.development` (development)
5. `.env`

## 📚 Reference Templates

### Complete Development Template

```env
# Database
DATABASE_URL="postgresql://postgres:password@localhost:5432/openforum"

# Authentication
BETTER_AUTH_SECRET="development-secret-key-32-characters-minimum"
BETTER_AUTH_URL="http://localhost:3000"

# Email
AUTH_RESEND_KEY="re_test_xxxxxxxxxxxxxxxxxxxxx"

# OAuth (optional)
GOOGLE_CLIENT_ID="your-dev-google-client-id"
GOOGLE_CLIENT_SECRET="your-dev-google-secret"
GITHUB_CLIENT_ID="your-dev-github-client-id"
GITHUB_CLIENT_SECRET="your-dev-github-secret"

# Application
APP_NAME="OpenForum Dev"
NEXT_PUBLIC_APP_URL="http://localhost:3000"
NODE_ENV="development"
```

### Complete Production Template

```env
# Database
DATABASE_URL="postgresql://username:password@production-host:5432/openforum"

# Authentication
BETTER_AUTH_SECRET="very-strong-production-secret-minimum-64-characters-recommended"
BETTER_AUTH_URL="https://yourdomain.com"

# Email
AUTH_RESEND_KEY="re_live_xxxxxxxxxxxxxxxxxxxxx"

# OAuth
GOOGLE_CLIENT_ID="your-prod-google-client-id"
GOOGLE_CLIENT_SECRET="your-prod-google-secret"
GITHUB_CLIENT_ID="your-prod-github-client-id"
GITHUB_CLIENT_SECRET="your-prod-github-secret"

# Application
APP_NAME="Your Forum Name"
NEXT_PUBLIC_APP_URL="https://yourdomain.com"
NODE_ENV="production"
```

This configuration system provides flexibility while maintaining security across different deployment environments.
