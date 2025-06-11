---
id: overview
title: Getting Started Overview
sidebar_label: Overview
---

# Getting Started with OpenForum

Welcome to OpenForum! This comprehensive guide will help you get your forum up and running quickly, whether you're setting up a development environment or deploying to production.

## 🎯 What You'll Learn

By the end of this guide, you'll have:
- ✅ A fully functional OpenForum installation
- ✅ User registration and authentication working
- ✅ Categories and discussions set up
- ✅ Admin panel access configured
- ✅ Email notifications enabled
- ✅ OAuth providers connected (optional)

## 📋 Prerequisites

Before you begin, ensure you have the following installed and ready:

### Required
- **Node.js** (version 18 or higher) - [Download here](https://nodejs.org/)
- **pnpm** (recommended) or npm - [Install pnpm](https://pnpm.io/installation)
- **PostgreSQL** database (local or cloud) - [Setup guide](https://www.postgresql.org/download/)
- **Git** for version control - [Download here](https://git-scm.com/)

### Optional but Recommended
- **Resend Account** - For email notifications [Sign up](https://resend.com)
- **Google/GitHub OAuth Apps** - For social login
- **Code Editor** - VS Code recommended with TypeScript support

## 🚀 Quick Setup Path

Choose your setup approach:

### 🔥 Express Setup (5 minutes)
Perfect for trying OpenForum quickly:
1. Clone repository
2. Install dependencies
3. Set up environment variables
4. Run with SQLite (development only)

### 🏗️ Full Setup (15 minutes)
Recommended for development and production:
1. Clone repository
2. Install dependencies  
3. Set up PostgreSQL database
4. Configure environment variables
5. Run database migrations
6. Configure email service
7. Set up OAuth providers (optional)

### ☁️ Cloud Deployment (10 minutes)
Deploy directly to production:
1. Fork repository
2. Deploy to Vercel/Railway
3. Connect cloud database
4. Configure environment variables
5. Set up domain

## 🛤️ Setup Steps Overview

### Step 1: Installation
- Clone the OpenForum repository
- Install all project dependencies
- Verify your development environment

### Step 2: Environment Configuration  
- Set up your `.env.local` file
- Configure database connection
- Set authentication secrets
- Configure email service

### Step 3: Database Setup
- Create your PostgreSQL database
- Run database migrations
- Understand the schema structure

### Step 4: First Steps
- Start the development server
- Create your first admin account
- Set up initial categories
- Test core functionality

## 🎯 Success Checklist

After completing the setup, verify these features work:

- [ ] **Homepage loads** - Forum homepage displays correctly
- [ ] **User registration** - New users can create accounts
- [ ] **Email verification** - Verification emails are sent and received
- [ ] **User login** - Authentication works with email/password
- [ ] **OAuth login** - Social login works (if configured)
- [ ] **Category creation** - Admin can create forum categories
- [ ] **Thread creation** - Users can create new discussion threads
- [ ] **Posting replies** - Users can reply to threads
- [ ] **Admin panel access** - Admin panel is accessible and functional
- [ ] **Email notifications** - Users receive notification emails

## 🆘 Need Help?

If you encounter issues during setup:

1. **Check the [FAQ](../support/faq)** - Common setup issues and solutions
2. **Review [Troubleshooting](../guides/troubleshooting)** - Detailed problem-solving guide
3. **Search [GitHub Issues](https://github.com/your-username/openforum/issues)** - See if others have faced similar issues
4. **Ask in [Discussions](https://github.com/your-username/openforum/discussions)** - Get help from the community

## 🔗 What's Next?

Choose your next step based on your goals:

### For Development
- **[Installation Guide](./installation)** - Detailed setup instructions
- **[Development Guide](../development/architecture)** - Understanding the codebase
- **[Contributing](../development/contributing)** - How to contribute to OpenForum

### For Production
- **[Deployment Guide](../deployment/overview)** - Production deployment options
- **[Configuration](../configuration/environment)** - Advanced configuration options
- **[Production Checklist](../deployment/production-checklist)** - Pre-launch checklist

### For Customization
- **[Theming Guide](../customization/theming)** - Customize the appearance
- **[Branding Guide](../customization/branding)** - Add your brand identity
- **[Component Customization](../customization/components)** - Modify UI components

---

Ready to dive in? Let's start with the **[Installation Guide](./installation)** 🚀
