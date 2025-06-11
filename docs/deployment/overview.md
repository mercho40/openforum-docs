---
id: overview
title: Deployment Overview
sidebar_label: Overview
---

# Deployment Overview

This guide covers deploying OpenForum to production environments. OpenForum is built with Next.js 15 and can be deployed to various platforms that support Node.js applications.

## 🎯 Deployment Options

### ☁️ Recommended Platforms

**Vercel (Recommended)**
- ✅ Built for Next.js applications
- ✅ Automatic deployments from Git
- ✅ Edge network for global performance
- ✅ Built-in SSL and custom domains
- ✅ Generous free tier

**Railway**
- ✅ Simple deployment process
- ✅ Built-in PostgreSQL database
- ✅ GitHub integration
- ✅ Automatic HTTPS
- ✅ Affordable pricing

**Netlify**
- ✅ Git-based deployments
- ✅ Built-in CI/CD
- ✅ Edge functions support
- ✅ Custom domains and SSL

### 🐳 Container Platforms

**Docker + Any Cloud Provider**
- ✅ Full control over environment
- ✅ Consistent across environments
- ✅ Works with any Docker-compatible platform
- ✅ Easy scaling and management

**AWS ECS/EKS**
- ✅ Enterprise-grade scaling
- ✅ Integration with AWS services
- ✅ Advanced networking options

**Google Cloud Run**
- ✅ Serverless container deployment
- ✅ Automatic scaling
- ✅ Pay-per-use pricing

### 🖥️ Traditional Hosting

**VPS/Dedicated Servers**
- ✅ Full control over server
- ✅ Custom configurations
- ✅ Cost-effective for high traffic
- ✅ Direct database access

## 📋 Pre-Deployment Checklist

### ✅ Environment Preparation

- [ ] **Production database** - PostgreSQL instance ready
- [ ] **Environment variables** - All required vars configured
- [ ] **Email service** - Resend account with verified domain
- [ ] **OAuth providers** - Production redirect URLs configured
- [ ] **Domain name** - DNS pointing to deployment platform
- [ ] **SSL certificate** - HTTPS configured (usually automatic)

### ✅ Application Preparation

- [ ] **Build successful** - `pnpm build` completes without errors
- [ ] **Database migrations** - Schema is up to date
- [ ] **Admin account** - First admin user plan ready
- [ ] **Content preparation** - Initial categories and guidelines ready
- [ ] **Email templates** - Customized for your brand

### ✅ Security Preparation

- [ ] **Strong secrets** - Production-grade authentication secrets
- [ ] **CORS configuration** - Proper domain restrictions
- [ ] **Rate limiting** - API protection configured
- [ ] **Error handling** - Proper error pages and logging
- [ ] **Backup strategy** - Database backup plan in place

## 🚀 Quick Deploy Options

### 1-Click Deploys

**Deploy to Vercel**
[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https://github.com/your-username/openforum)

**Deploy to Railway**
[![Deploy on Railway](https://railway.app/button.svg)](https://railway.app/template/your-template-id)

**Deploy to Netlify**
[![Deploy to Netlify](https://www.netlify.com/img/deploy/button.svg)](https://app.netlify.com/start/deploy?repository=https://github.com/your-username/openforum)

### Manual Deploy Steps

1. **Fork the repository** to your GitHub account
2. **Choose your platform** and connect your repository
3. **Configure environment variables** in platform dashboard
4. **Set up database** (cloud PostgreSQL recommended)
5. **Deploy and verify** functionality

## 🗄️ Database Options

### Cloud PostgreSQL Providers

**Neon (Recommended)**
- ✅ Serverless PostgreSQL
- ✅ Automatic scaling
- ✅ Generous free tier
- ✅ Built-in connection pooling
- [Sign up at neon.tech](https://neon.tech)

**Supabase**
- ✅ PostgreSQL with additional features
- ✅ Built-in auth and storage
- ✅ Real-time subscriptions
- ✅ Dashboard for database management
- [Sign up at supabase.com](https://supabase.com)

**PlanetScale**
- ✅ MySQL-compatible (requires schema changes)
- ✅ Branching for database schema
- ✅ Global replication
- [Sign up at planetscale.com](https://planetscale.com)

**Railway PostgreSQL**
- ✅ Integrated with Railway deployments
- ✅ Automatic backups
- ✅ Simple setup
- [Available in Railway dashboard](https://railway.app)

### Self-Hosted Database

**Managed Cloud Instances**
- AWS RDS PostgreSQL
- Google Cloud SQL
- Azure Database for PostgreSQL
- DigitalOcean Managed Databases

**VPS/Server Setup**
- Install PostgreSQL on your server
- Configure security and backups
- Set up connection pooling
- Monitor performance

## 📧 Email Service Setup

### Resend (Recommended)

1. **Create account** at [resend.com](https://resend.com)
2. **Verify your domain**:
   - Add your domain in dashboard
   - Add DNS records provided
   - Wait for verification
3. **Generate API key** for production
4. **Update environment variables**

### Alternative Email Providers

**SendGrid**
- Enterprise-grade email delivery
- Advanced analytics and reporting
- Template management

**Mailgun**
- Developer-friendly API
- Email validation
- EU/US data residency options

**Amazon SES**
- Cost-effective for high volume
- Integration with AWS services
- Detailed sending statistics

## 🔒 Security Considerations

### Environment Security

```env
# Use strong, unique secrets
BETTER_AUTH_SECRET="minimum-64-characters-cryptographically-secure-random-string"

# Use production URLs
BETTER_AUTH_URL="https://yourdomain.com"
NEXT_PUBLIC_APP_URL="https://yourdomain.com"

# Secure database connection
DATABASE_URL="postgresql://user:password@secure-host:5432/db?sslmode=require"
```

### Application Security

- **HTTPS Only** - Force HTTPS redirects
- **Secure Headers** - CSP, HSTS, X-Frame-Options
- **Rate Limiting** - Protect API endpoints
- **Input Validation** - Sanitize user input
- **Error Handling** - Don't expose sensitive info

### Database Security

- **Connection Encryption** - Use SSL/TLS
- **Access Control** - Limit database user permissions
- **Network Security** - VPC/firewall configuration
- **Regular Backups** - Automated backup strategy
- **Monitoring** - Database performance and security alerts

## 📊 Monitoring and Analytics

### Application Monitoring

**Error Tracking**
- Sentry for error monitoring
- LogRocket for session replay
- Custom error logging

**Performance Monitoring**
- Vercel Analytics (if using Vercel)
- Google Analytics
- Custom performance metrics

**Uptime Monitoring**
- UptimeRobot
- Pingdom
- StatusCake

### Database Monitoring

- Connection pool usage
- Query performance
- Storage usage
- Backup success/failure

## 🔄 CI/CD Pipeline

### Automated Deployment

**GitHub Actions Example**
```yaml
name: Deploy to Production

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: 18
      - run: npm install -g pnpm
      - run: pnpm install
      - run: pnpm build
      - run: pnpm test
      - name: Deploy to Vercel
        uses: amondnet/vercel-action@v20
        with:
          vercel-token: ${{ secrets.VERCEL_TOKEN }}
          vercel-org-id: ${{ secrets.ORG_ID }}
          vercel-project-id: ${{ secrets.PROJECT_ID }}
```

### Deployment Stages

1. **Development** - Feature development
2. **Staging** - Testing with production-like data
3. **Production** - Live application

## 🎯 Platform-Specific Guides

Choose your deployment platform:

### Cloud Platforms
- **[Vercel Deployment](./vercel)** - Step-by-step Vercel setup
- **[Railway Deployment](./railway)** - Complete Railway guide
- **[Netlify Deployment](./netlify)** - Netlify configuration

### Container Deployment
- **[Docker Deployment](./docker)** - Docker setup and configuration
- **[AWS ECS Deployment](./aws-ecs)** - Amazon container service
- **[Google Cloud Run](./google-cloud-run)** - Serverless containers

### Self-Hosted
- **[VPS Deployment](./self-hosted)** - Linux server setup
- **[DigitalOcean Droplet](./digitalocean)** - Complete droplet guide
- **[AWS EC2 Deployment](./aws-ec2)** - EC2 instance setup

## 🚨 Troubleshooting Deployment

### Common Issues

**Build Failures**
- Check Node.js version compatibility
- Verify all dependencies are installed
- Review TypeScript compilation errors

**Database Connection Issues**
- Verify DATABASE_URL format
- Check firewall/security group settings
- Confirm SSL requirements

**Environment Variable Problems**
- Ensure all required variables are set
- Check for typos in variable names
- Verify secret lengths and formats

**Email Service Issues**
- Confirm API key validity
- Check domain verification status
- Review DNS record configuration

### Getting Help

- **[Production Checklist](./production-checklist)** - Pre-launch verification
- **[Troubleshooting Guide](../guides/troubleshooting)** - Common solutions
- **[Community Support](../support/community)** - Get help from others
- **[GitHub Issues](https://github.com/your-username/openforum/issues)** - Report bugs

---

Ready to deploy? Choose your platform and follow the detailed deployment guide! 🚀
