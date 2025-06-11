---
id: installation
title: Installation Guide
sidebar_label: Installation
---

# Installation Guide

This guide will walk you through installing OpenForum step by step. Follow these instructions to get your forum running locally for development or testing.

## 🚀 Quick Installation

### 1. Clone the Repository

First, clone the OpenForum repository to your local machine:

```bash
# Clone the repository
git clone https://github.com/your-username/openforum.git

# Navigate to the project directory
cd openforum
```

### 2. Install Dependencies

OpenForum uses **pnpm** as the preferred package manager for better performance and disk efficiency:

```bash
# Install pnpm if you haven't already
npm install -g pnpm

# Install project dependencies
pnpm install
```

**Alternative with npm:**
```bash
# If you prefer npm
npm install
```

### 3. Environment Setup

Create your environment configuration file:

```bash
# Copy the example environment file
cp .env.example .env.local

# Or create a new .env.local file
touch .env.local
```

Add the following essential environment variables to your `.env.local` file:

```env
# Database Configuration
DATABASE_URL="postgresql://username:password@localhost:5432/openforum"

# Authentication (generate a secure random string)
BETTER_AUTH_SECRET="your-super-secret-key-here-minimum-32-characters"
BETTER_AUTH_URL="http://localhost:3000"

# Email Service (Required for user registration)
AUTH_RESEND_KEY="re_xxxxxxxxxxxxxxxxxxxxx"

# Application Configuration
APP_NAME="OpenForum"
NEXT_PUBLIC_APP_URL="http://localhost:3000"
```

:::tip Generating a Secure Auth Secret
Generate a secure random string for `BETTER_AUTH_SECRET`:
```bash
# Generate a 32-character random string
openssl rand -base64 32
```
:::

### 4. Database Setup

#### Option A: Local PostgreSQL

1. **Install PostgreSQL** (if not already installed):
   ```bash
   # macOS with Homebrew
   brew install postgresql
   brew services start postgresql
   
   # Ubuntu/Debian
   sudo apt update
   sudo apt install postgresql postgresql-contrib
   
   # Windows - Download from postgresql.org
   ```

2. **Create Database**:
   ```bash
   # Create a new database
   createdb openforum
   
   # Or connect to PostgreSQL and create manually
   psql postgres
   CREATE DATABASE openforum;
   \q
   ```

3. **Update Connection String**:
   ```env
   DATABASE_URL="postgresql://your-username:your-password@localhost:5432/openforum"
   ```

#### Option B: Cloud Database (Recommended for Production)

Choose a cloud PostgreSQL provider:

**Neon (Recommended)**
1. Sign up at [neon.tech](https://neon.tech)
2. Create a new project
3. Copy the connection string
4. Update `DATABASE_URL` in your `.env.local`

**Supabase**
1. Sign up at [supabase.com](https://supabase.com)
2. Create a new project
3. Go to Settings → Database
4. Copy the connection string
5. Update `DATABASE_URL` in your `.env.local`

**Railway**
1. Sign up at [railway.app](https://railway.app)
2. Create a new PostgreSQL service
3. Copy the connection string
4. Update `DATABASE_URL` in your `.env.local`

### 5. Run Database Migrations

Generate and apply the database schema:

```bash
# Generate migration files (if not already present)
pnpm drizzle-kit generate

# Apply migrations to your database
pnpm drizzle-kit migrate
```

### 6. Start Development Server

Launch the development server:

```bash
# Start the development server
pnpm dev
```

Your forum will be available at [http://localhost:3000](http://localhost:3000) 🎉

## 📧 Email Service Setup

Email is required for user registration and notifications. OpenForum uses [Resend](https://resend.com) for reliable email delivery.

### 1. Create Resend Account

1. Sign up at [resend.com](https://resend.com)
2. Verify your email address
3. Go to API Keys in your dashboard
4. Create a new API key
5. Copy the key (starts with `re_`)

### 2. Configure Environment

Add your Resend API key to `.env.local`:

```env
AUTH_RESEND_KEY="re_xxxxxxxxxxxxxxxxxxxxx"
```

### 3. Test Email Functionality

1. Start your development server
2. Try registering a new user
3. Check that verification emails are sent
4. Verify the email verification process works

:::warning Domain Verification for Production
For production use, you'll need to verify your domain in Resend:
1. Add your domain in the Resend dashboard
2. Add the required DNS records
3. Wait for verification (usually takes a few minutes)
:::

## 🔐 OAuth Setup (Optional)

Enable social login with Google and GitHub:

### Google OAuth

1. **Go to [Google Cloud Console](https://console.cloud.google.com)**
2. **Create a new project** or select existing
3. **Enable Google+ API**:
   - Go to APIs & Services → Library
   - Search for "Google+ API" and enable it
4. **Create OAuth 2.0 credentials**:
   - Go to APIs & Services → Credentials
   - Create credentials → OAuth 2.0 Client IDs
   - Application type: Web application
   - Authorized redirect URIs: `http://localhost:3000/api/auth/callback/google`
5. **Copy credentials** and add to `.env.local`:

```env
GOOGLE_CLIENT_ID="your-google-client-id"
GOOGLE_CLIENT_SECRET="your-google-client-secret"
```

### GitHub OAuth

1. **Go to GitHub Settings** → Developer settings → OAuth Apps
2. **Create a new OAuth App**:
   - Application name: Your Forum Name
   - Homepage URL: `http://localhost:3000`
   - Authorization callback URL: `http://localhost:3000/api/auth/callback/github`
3. **Copy credentials** and add to `.env.local`:

```env
GITHUB_CLIENT_ID="your-github-client-id"
GITHUB_CLIENT_SECRET="your-github-client-secret"
```

## 🔧 Development Tools

### Database Management

**Drizzle Studio** - Visual database browser:
```bash
# Open Drizzle Studio
pnpm drizzle-kit studio
```

Access at [http://localhost:4983](http://localhost:4983)

### Code Quality Tools

**Linting**:
```bash
# Run ESLint
pnpm lint

# Fix linting issues automatically
pnpm lint --fix
```

**Type Checking**:
```bash
# Run TypeScript type checking
pnpm type-check
```

### Building for Production

```bash
# Build the application
pnpm build

# Start production server (after build)
pnpm start
```

## 📁 Project Structure Overview

Understanding the project structure:

```
openforum/
├── app/                    # Next.js 15 App Router
│   ├── api/               # API routes and authentication
│   ├── auth/              # Authentication pages
│   ├── forum/             # Main forum pages
│   └── layout.tsx         # Root layout
├── components/            # React components
│   ├── auth/              # Authentication forms
│   ├── forum/             # Forum-specific components
│   ├── ui/                # Reusable UI components
│   └── views/             # Page-level view components
├── db/                    # Database configuration
│   ├── schema.ts          # Drizzle database schema
│   └── drizzle.ts         # Database connection
├── lib/                   # Utility libraries
│   ├── auth.ts            # Better Auth configuration
│   ├── auth-client.ts     # Client-side auth utilities
│   └── utils.ts           # Helper functions
├── actions/               # Server actions for data operations
├── migrations/            # Database migration files
└── docs/                  # Documentation files
```

## 🚨 Common Installation Issues

### Database Connection Issues

**Error: `connection refused`**
```bash
# Check if PostgreSQL is running
brew services list | grep postgresql

# Start PostgreSQL if not running
brew services start postgresql
```

**Error: `database does not exist`**
```bash
# Create the database
createdb openforum
```

**Error: `authentication failed`**
- Check your username and password in `DATABASE_URL`
- Ensure the user has proper permissions

### Node.js Version Issues

**Error: `unsupported Node.js version`**
```bash
# Check your Node.js version
node --version

# Update Node.js if needed (using nvm)
nvm install 18
nvm use 18
```

### Permission Issues

**Error: `EACCES` permission errors**
```bash
# Fix npm permissions (macOS/Linux)
sudo chown -R $(whoami) ~/.npm

# Or reinstall Node.js with proper permissions
```

## ✅ Verification Checklist

After installation, verify these features work:

- [ ] **Development server starts** without errors
- [ ] **Homepage loads** at http://localhost:3000
- [ ] **Database connection** works (check logs)
- [ ] **User registration** form appears
- [ ] **Email verification** emails are sent (if configured)
- [ ] **OAuth login** buttons appear (if configured)
- [ ] **Admin panel** is accessible at `/forum/admin`

## 🔗 Next Steps

Now that OpenForum is installed:

1. **[First Steps Guide](./first-steps)** - Set up your first admin account and categories
2. **[Configuration Guide](../configuration/environment)** - Advanced configuration options
3. **[Deployment Guide](../deployment/overview)** - Deploy to production

---

**Installation complete!** 🎉 Ready to set up your forum? Continue to **[First Steps](./first-steps)**.
