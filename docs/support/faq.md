---
id: faq
title: Frequently Asked Questions
sidebar_label: FAQ
---

# Frequently Asked Questions

Find answers to the most common questions about OpenForum setup, configuration, and usage.

## 🚀 Getting Started

### Q: What are the system requirements for OpenForum?

**A:** OpenForum requires:

- **Node.js**: Version 18 or higher
- **Database**: PostgreSQL (local or cloud)
- **Package Manager**: pnpm (recommended) or npm
- **Operating System**: Windows, macOS, or Linux

For production deployment, you'll also need:

- Cloud hosting platform (Vercel, Railway, etc.)
- Email service (Resend recommended)
- Domain name (optional but recommended)

### Q: How long does it take to set up OpenForum?

**A:** Setup time depends on your approach:

- **Quick local setup**: 10-15 minutes
- **Full development setup**: 30-45 minutes
- **Production deployment**: 1-2 hours (including domain configuration)

The process is straightforward with our step-by-step guides.

### Q: Can I try OpenForum without setting up a database?

**A:** For quick testing, you can:

1. Use the provided demo or sandbox environment
2. Deploy to a platform that provides a database (like Railway)
3. Use a cloud database service like Neon's free tier

However, for development or production, you'll need a proper PostgreSQL database.

## 💾 Database Questions

### Q: Which database providers are recommended?

**A:** For cloud databases, we recommend:

- **Neon** (PostgreSQL, generous free tier, serverless)
- **Supabase** (PostgreSQL with additional features)
- **Railway** (PostgreSQL, simple setup)
- **PlanetScale** (MySQL, requires schema modifications)

For local development:

- **Local PostgreSQL** installation
- **Docker PostgreSQL** container

### Q: How do I migrate from another forum platform?

**A:** Migration depends on your current platform:

**From Discourse:**

1. Export data using Discourse API
2. Transform data to OpenForum schema
3. Use custom migration scripts
4. Import user accounts and content

**From phpBB/vBulletin:**

1. Export database dumps
2. Map schema differences
3. Convert user authentication
4. Import content with proper relationships

We're working on migration tools for common platforms. Contact the community for assistance with specific migrations.

### Q: What happens if I run out of database storage?

**A:** Most cloud providers offer automatic scaling:

- **Neon**: Automatically scales within plan limits
- **Supabase**: Upgrade plan for more storage
- **Railway**: Volume expansion available

For self-hosted databases:

1. Monitor storage usage regularly
2. Archive old content if needed
3. Optimize database with cleanup scripts
4. Upgrade server storage capacity

## 🔐 Authentication & Security

### Q: How do I reset my admin password?

**A:** If you're locked out of your admin account:

**Option 1: Database Reset**

```sql
-- Connect to your database
psql "your-database-url"

-- Reset password (requires hashing)
UPDATE "user" SET password = 'new-hashed-password' WHERE email = 'admin@example.com';
```

**Option 2: Create New Admin**

```sql
-- Create new admin user
INSERT INTO "user" (id, email, name, role) VALUES 
('admin-id', 'newadmin@example.com', 'New Admin', 'admin');
```

**Option 3: Use Recovery Method**

1. Use the "Forgot Password" feature
2. Access the recovery email
3. Reset through the provided link

### Q: How do I set up two-factor authentication?

**A:** OpenForum supports 2FA through Better Auth:

1. **Enable in Configuration**:

   ```typescript
   // lib/auth.ts
   plugins: [
     twoFactor({
       issuer: "OpenForum",
       totpWindow: 1,
     }),
     // ... other plugins
   ]
   ```

2. **User Setup**:
   - Go to Profile Settings
   - Enable Two-Factor Authentication
   - Scan QR code with authenticator app
   - Enter verification code to confirm

3. **Backup Codes**:
   - Generate backup codes during setup
   - Store securely for account recovery

### Q: Can I disable user registration?

**A:** Yes, you can control registration in several ways:

**Environment Variable**:

```env
ALLOW_REGISTRATION=false
```

**Code Configuration**:

```typescript
// lib/auth.ts
emailAndPassword: {
  enabled: true,
  requireEmailVerification: true,
  autoSignIn: false,
}
```

**Admin Panel**:

- Go to Admin → Settings
- Toggle "Allow New Registrations"
- Set registration approval requirements

## 📧 Email Configuration

### Q: Email verification isn't working. What should I check?

**A:** Common email issues and solutions:

**Check API Key**:

```bash
# Verify Resend API key format
echo $AUTH_RESEND_KEY  # Should start with "re_"
```

**Verify Domain**:

1. Check domain verification in Resend dashboard
2. Confirm DNS records are properly set
3. Wait for DNS propagation (up to 24 hours)

**Test Email Sending**:

```typescript
// Test email function
import { sendVerificationEmail } from '@/actions/email';

await sendVerificationEmail({
  email: 'test@yourdomain.com',
  otp: '123456',
  type: 'email-verification'
});
```

**Check Spam Folders**:

- Verification emails might be filtered
- Whitelist your domain with users
- Configure SPF/DKIM records for better deliverability

### Q: Can I use a different email provider?

**A:** Yes, you can integrate other email services:

**SendGrid Integration**:

```typescript
// actions/email.ts
import sgMail from '@sendgrid/mail';

sgMail.setApiKey(process.env.SENDGRID_API_KEY);

export async function sendEmail({ to, subject, html }) {
  await sgMail.send({
    to,
    from: 'noreply@yourdomain.com',
    subject,
    html,
  });
}
```

**Nodemailer (SMTP)**:

```typescript
import nodemailer from 'nodemailer';

const transporter = nodemailer.createTransporter({
  host: process.env.SMTP_HOST,
  port: 587,
  auth: {
    user: process.env.SMTP_USER,
    pass: process.env.SMTP_PASS,
  },
});
```

## 🎨 Customization

### Q: How do I change the forum theme/colors?

**A:** OpenForum uses Tailwind CSS for styling:

**CSS Variables** (recommended):

```css
/* app/globals.css */
:root {
  --primary: 210 40% 98%;
  --primary-foreground: 222.2 84% 4.9%;
  /* ... other color variables */
}

.dark {
  --primary: 222.2 84% 4.9%;
  --primary-foreground: 210 40% 98%;
  /* ... dark mode colors */
}
```

**Tailwind Config**:

```javascript
// tailwind.config.js
module.exports = {
  theme: {
    extend: {
      colors: {
        primary: {
          50: '#eff6ff',
          500: '#3b82f6',
          900: '#1e3a8a',
        }
      }
    }
  }
}
```

**Component Customization**:

- Modify components in `components/ui/`
- Update theme in `components.json`
- Use CSS modules for component-specific styles

### Q: How do I add custom pages?

**A:** Add new pages using Next.js App Router:

**Create Page File**:

```typescript
// app/custom-page/page.tsx
export default function CustomPage() {
  return (
    <div>
      <h1>Custom Page</h1>
      <p>Your custom content here</p>
    </div>
  );
}
```

**Add Navigation Link**:

```typescript
// components/Navigation.tsx
<Link href="/custom-page">
  Custom Page
</Link>
```

**Add to Admin Menu** (if needed):

```typescript
// app/forum/admin/layout.tsx
const navItems = [
  // ... existing items
  { href: "/forum/admin/custom", label: "Custom Feature" },
];
```

### Q: Can I modify the database schema?

**A:** Yes, but follow these steps carefully:

**Create Migration**:

```bash
# Modify db/schema.ts with your changes
# Generate migration
pnpm drizzle-kit generate

# Review the generated SQL
# Apply migration
pnpm drizzle-kit migrate
```

**Example Schema Addition**:

```typescript
// db/schema.ts
export const customTable = pgTable("custom_table", {
  id: text('id').primaryKey(),
  name: text('name').notNull(),
  createdAt: timestamp('created_at').defaultNow(),
});
```

**Update TypeScript Types**:

```typescript
// types/index.ts
export type CustomType = InferSelectModel<typeof customTable>;
```

## 🚀 Deployment

### Q: My deployment fails on Vercel. What should I check?

**A:** Common Vercel deployment issues:

**Build Errors**:

```bash
# Check build locally first
pnpm build

# Common issues:
# - Missing environment variables
# - TypeScript errors
# - Import path issues
```

**Environment Variables**:

- Ensure all required variables are set in Vercel dashboard
- Check variable names for typos
- Verify secret lengths and formats

**Database Connection**:

```env
# Ensure SSL mode for cloud databases
DATABASE_URL="postgresql://user:pass@host:5432/db?sslmode=require"
```

**Memory Limits**:

- Upgrade Vercel plan if hitting memory limits
- Optimize bundle size
- Use dynamic imports for large components

### Q: How do I set up a custom domain?

**A:** Domain setup varies by platform:

**Vercel**:

1. Go to Project Settings → Domains
2. Add your domain
3. Configure DNS with CNAME record: `cname.vercel-dns.com`
4. Update environment variables with new domain

**Railway**:

1. Go to Project Settings → Domains
2. Add custom domain
3. Configure DNS with provided values
4. Enable HTTPS

**Cloudflare** (for additional security):

1. Add domain to Cloudflare
2. Configure DNS records
3. Enable security features
4. Use flexible SSL mode

### Q: How do I enable HTTPS?

**A:** HTTPS setup depends on your hosting:

**Vercel/Railway/Netlify**:

- HTTPS is automatic with custom domains
- Certificates are provisioned automatically
- No additional configuration needed

**Self-hosted**:

```nginx
# nginx configuration
server {
    listen 443 ssl;
    ssl_certificate /path/to/certificate.crt;
    ssl_certificate_key /path/to/private.key;
    
    location / {
        proxy_pass http://localhost:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

**Let's Encrypt** (free certificates):

```bash
# Install certbot
sudo apt install certbot python3-certbot-nginx

# Get certificate
sudo certbot --nginx -d yourdomain.com
```

## ⚙️ Administration

### Q: How do I backup my forum data?

**A:** Backup strategies by component:

**Database Backup**:

```bash
# PostgreSQL dump
pg_dump "your-database-url" > forum_backup_$(date +%Y%m%d).sql

# Compressed backup
pg_dump "your-database-url" | gzip > forum_backup_$(date +%Y%m%d).sql.gz
```

**File Backup**:

```bash
# Application files
tar -czf forum_files_$(date +%Y%m%d).tar.gz /path/to/forum

# Environment variables (excluding secrets)
cp .env.local .env.backup
```

**Automated Backup**:

```bash
#!/bin/bash
# backup.sh
DATE=$(date +%Y%m%d_%H%M%S)
pg_dump "$DATABASE_URL" | gzip > "backups/forum_${DATE}.sql.gz"
# Upload to cloud storage
aws s3 cp "backups/forum_${DATE}.sql.gz" s3://your-backup-bucket/
```

### Q: How do I moderate content effectively?

**A:** Content moderation best practices:

**Set Clear Guidelines**:

1. Create comprehensive community guidelines
2. Pin guidelines in prominent categories
3. Reference guidelines in registration process

**Use Admin Tools**:

- Review reported content promptly
- Use bulk moderation for spam
- Pin important announcements
- Lock threads when necessary

**Build Moderation Team**:

```sql
-- Promote trusted users to moderators
UPDATE "user" SET role = 'moderator' WHERE id = 'user-id';
```

**Automate When Possible**:

- Set up keyword filters
- Use reputation-based permissions
- Implement rate limiting
- Monitor for spam patterns

### Q: How do I handle spam attacks?

**A:** Anti-spam strategies:

**Immediate Response**:

1. Ban offending accounts immediately
2. Delete spam content in bulk
3. Implement IP-based blocking
4. Enable CAPTCHA for registration

**Preventive Measures**:

```typescript
// Rate limiting example
export async function createPost(data: PostData) {
  const rateLimitKey = `post:${userId}`;
  const attempts = await redis.incr(rateLimitKey);
  
  if (attempts > 5) {
    throw new Error("Rate limit exceeded");
  }
  
  await redis.expire(rateLimitKey, 300); // 5 minutes
  // ... create post
}
```

**Long-term Solutions**:

- Require email verification
- Implement reputation requirements
- Use content filters
- Monitor registration patterns

## 🔧 Technical Issues

### Q: The forum is running slowly. How can I optimize performance?

**A:** Performance optimization checklist:

**Database Optimization**:

```sql
-- Add missing indexes
CREATE INDEX CONCURRENTLY idx_thread_category_lastpost 
ON thread(category_id, last_post_at DESC);

-- Analyze query performance
EXPLAIN ANALYZE SELECT * FROM thread WHERE category_id = 'cat-id';
```

**Application Optimization**:

- Enable Next.js caching
- Optimize image loading
- Use React Server Components
- Implement proper pagination

**Infrastructure Optimization**:

- Use CDN for static assets
- Enable database connection pooling
- Monitor server resources
- Upgrade hosting plan if needed

### Q: I'm getting database connection errors. What should I check?

**A:** Database connection troubleshooting:

**Connection String**:

```bash
# Test connection manually
psql "your-database-url" -c "SELECT 1;"
```

**Common Issues**:

1. **SSL Requirements**: Add `?sslmode=require`
2. **Connection Limits**: Check database plan limits
3. **Network Issues**: Verify firewall settings
4. **Credentials**: Confirm username/password

**Connection Pooling**:

```typescript
// db/drizzle.ts
import { Pool } from 'pg';

const pool = new Pool({
  connectionString: process.env.DATABASE_URL,
  max: 20, // Maximum connections
  idleTimeoutMillis: 30000,
  connectionTimeoutMillis: 2000,
});
```

### Q: How do I troubleshoot OAuth login issues?

**A:** OAuth troubleshooting steps:

**Verify Configuration**:

```env
# Check environment variables
GOOGLE_CLIENT_ID=correct-client-id
GOOGLE_CLIENT_SECRET=correct-secret
BETTER_AUTH_URL=https://yourdomain.com  # Must match OAuth config
```

**Check Redirect URLs**:

- Google Console: `https://yourdomain.com/api/auth/callback/google`
- GitHub Settings: `https://yourdomain.com/api/auth/callback/github`
- Must match exactly (HTTP vs HTTPS, www vs non-www)

**Debug OAuth Flow**:

1. Check browser network tab for errors
2. Verify OAuth app is active
3. Test with different browsers/incognito
4. Check OAuth provider status pages

**Common Solutions**:

- Clear browser cache and cookies
- Verify OAuth app permissions
- Check domain verification
- Update redirect URLs for new domains

## 🆘 Getting More Help

### Q: Where can I get additional support?

**A:** Support resources:

**Documentation**:

- [Complete documentation](/) with detailed guides
- [API reference](/development/api-reference) for developers
- [Troubleshooting guide](/guides/troubleshooting) for common issues

**Community Support**:

- [GitHub Discussions](https://github.com/mercho40/openforum/discussions) for questions
- [GitHub Issues](https://github.com/your-username/openforum/issues) for bug reports
- [Discord Community](https://discord.gg/your-server) for real-time chat

**Professional Support**:

- Custom development services
- Migration assistance
- Performance optimization
- Enterprise support packages

### Q: How do I report a bug or request a feature?

**A:** Contributing to OpenForum:

**Bug Reports**:

1. Check existing issues first
2. Use the bug report template
3. Include reproduction steps
4. Provide error messages and logs
5. Specify environment details

**Feature Requests**:

1. Search existing feature requests
2. Use the feature request template
3. Describe the use case clearly
4. Explain expected behavior
5. Consider implementation complexity

**Contributing Code**:

1. Fork the repository
2. Create a feature branch
3. Follow coding standards
4. Add tests for new features
5. Submit a pull request

---

**Still have questions?** Check our [community discussions](https://github.com/mercho40/openforum/discussions) or [open a new issue](https://github.com/mercho40/openforum/issues) on GitHub!
