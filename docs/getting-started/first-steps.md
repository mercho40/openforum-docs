---
id: first-steps
title: First Steps
sidebar_label: First Steps
---

# First Steps After Installation

Congratulations on successfully installing OpenForum! This guide will walk you through the essential first steps to get your forum ready for users.

## 🎯 What We'll Cover

- Creating your first admin account
- Setting up forum categories
- Configuring basic forum settings
- Testing core functionality
- Inviting your first users

## 👤 Create Your Admin Account

### 1. Register the First User

1. **Navigate to your forum**: Open [http://localhost:3000](http://localhost:3000) in your browser
2. **Click "Sign Up"** in the top navigation
3. **Fill out the registration form**:
   - Use a real email address (you'll need to verify it)
   - Choose a strong password
   - Complete your profile information

4. **Verify your email** (if email service is configured):
   - Check your email for a verification message
   - Click the verification link or enter the OTP code
   - Complete the email verification process

### 2. Grant Admin Privileges

The first registered user needs to be manually promoted to admin:

:::info Database Access Required
You'll need access to your PostgreSQL database to complete this step. You can use any database client, psql command line, or Drizzle Studio.
:::

**Option A: Using psql (Command Line)**
```bash
# Connect to your database
psql "postgresql://username:password@localhost:5432/openforum"

# Update the user's role to admin
UPDATE "user" SET role = 'admin' WHERE email = 'your-email@example.com';

# Verify the change
SELECT name, email, role FROM "user" WHERE email = 'your-email@example.com';

# Exit psql
\q
```

**Option B: Using Drizzle Studio**
```bash
# Open Drizzle Studio
pnpm drizzle-kit studio
```

1. Navigate to [http://localhost:4983](http://localhost:4983)
2. Click on the "user" table
3. Find your user record
4. Edit the "role" field to "admin"
5. Save the changes

### 3. Verify Admin Access

1. **Log out and log back in** to refresh your session
2. **Navigate to the admin panel**: [http://localhost:3000/forum/admin](http://localhost:3000/forum/admin)
3. **Verify access**: You should see the admin dashboard

## 📁 Set Up Forum Categories

Categories help organize discussions and make your forum easier to navigate.

### 1. Access Category Management

1. **Go to the admin panel**: `/forum/admin`
2. **Click "Categories"** in the sidebar
3. **Click "New Category"** to create your first category

### 2. Create Essential Categories

Here are some recommended starter categories:

**General Discussion**
- **Name**: General Discussion
- **Description**: Open discussions about any topic
- **Icon**: `MessageSquare` (or choose from icon picker)
- **Color**: `#3498db` (blue)

**Help & Support**
- **Name**: Help & Support  
- **Description**: Get help and support from the community
- **Icon**: `HelpCircle`
- **Color**: `#e74c3c` (red)

**Announcements**
- **Name**: Announcements
- **Description**: Important updates and news
- **Icon**: `Megaphone`
- **Color**: `#f39c12` (orange)

**Feature Requests**
- **Name**: Feature Requests
- **Description**: Suggest new features and improvements
- **Icon**: `Lightbulb`
- **Color**: `#9b59b6` (purple)

### 3. Category Configuration Tips

- **Use descriptive names**: Make it clear what belongs in each category
- **Choose appropriate icons**: Lucide React icons help users quickly identify categories
- **Pick distinct colors**: Use different colors to make categories visually distinct
- **Write helpful descriptions**: Explain what type of content belongs in each category
- **Set display order**: Arrange categories in logical order (most important first)

## ⚙️ Configure Basic Forum Settings

### 1. Update Application Name

Edit your `.env.local` file to customize your forum name:

```env
APP_NAME="Your Forum Name"
NEXT_PUBLIC_APP_URL="http://localhost:3000"  # Update for production
```

### 2. Test Email Functionality

If you configured email service during installation:

1. **Test user registration**: Try registering a new test account
2. **Check email delivery**: Verify verification emails are sent and received
3. **Test password reset**: Use the "Forgot Password" feature
4. **Check notification emails**: Create a test thread and verify notifications

### 3. Configure OAuth (If Applicable)

If you set up OAuth providers:

1. **Test Google login**: Try signing in with Google
2. **Test GitHub login**: Try signing in with GitHub
3. **Verify profile creation**: Check that OAuth users get proper profiles
4. **Test account linking**: Verify users can link multiple auth methods

## 🧪 Test Core Functionality

### 1. Create Test Content

**Create a Welcome Thread**:
1. Navigate to one of your categories
2. Click "New Thread"
3. Title: "Welcome to [Your Forum Name]!"
4. Content: Write a welcoming message for new users
5. Pin the thread (if you're admin)

**Test Posting and Replies**:
1. Create a test thread with some discussion questions
2. Reply to the thread from your admin account
3. Test the voting system (upvote/downvote)
4. Test editing posts
5. Test thread moderation features (pin, lock, hide)

### 2. User Flow Testing

**Registration Flow**:
- [ ] New user registration works
- [ ] Email verification works (if configured)
- [ ] Profile completion works
- [ ] User can log in successfully

**Discussion Flow**:
- [ ] Users can view categories
- [ ] Users can view threads
- [ ] Users can create new threads
- [ ] Users can reply to threads
- [ ] Users can vote on posts
- [ ] Users can bookmark threads

**Admin Flow**:
- [ ] Admin panel is accessible
- [ ] Category management works
- [ ] Thread moderation works
- [ ] User management works

## 👥 Invite Your First Users

### 1. Prepare for Users

Before inviting users, ensure:
- [ ] Categories are set up and organized
- [ ] Welcome/guidelines thread is created and pinned
- [ ] Basic forum rules are established
- [ ] Email notifications are working
- [ ] Registration process is smooth

### 2. Create Guidelines Content

Create essential threads:

**Forum Guidelines** (in General or Announcements):
```markdown
# Forum Guidelines

Welcome to our community! Please follow these guidelines:

## Be Respectful
- Treat all members with respect and courtesy
- No harassment, hate speech, or personal attacks
- Disagree with ideas, not people

## Stay On Topic
- Post in the appropriate category
- Keep discussions relevant to the thread topic
- Use clear, descriptive thread titles

## Quality Content
- Search before posting to avoid duplicates
- Provide helpful, constructive feedback
- Use proper formatting and grammar

## Moderation
- Report inappropriate content using the report button
- Moderators may edit, move, or remove content as needed
- Respect moderator decisions

Thank you for helping make this a great community!
```

**FAQ Thread**:
Create common questions and answers about using the forum.

### 3. Invitation Methods

**Direct Invitations**:
- Share the forum URL with trusted users
- Provide them with the registration link
- Give them a brief orientation about the forum

**Soft Launch**:
- Start with a small group of trusted users
- Gather feedback and make improvements
- Gradually expand the user base

## 🔧 Development vs Production Notes

### Development Environment
- Use `http://localhost:3000` for all URLs
- SQLite database is fine for testing
- Email verification can be skipped for development
- OAuth redirect URLs should point to localhost

### Production Preparation
When ready for production:
- [ ] Set up production database (PostgreSQL)
- [ ] Configure production domain in environment variables
- [ ] Set up email service with verified domain
- [ ] Update OAuth redirect URLs to production domain
- [ ] Enable SSL/HTTPS
- [ ] Set up monitoring and backups

## 🎉 Success Checklist

Verify your forum is ready:

### Technical Setup
- [ ] Forum loads without errors
- [ ] User registration and login work
- [ ] Email verification works (if configured)
- [ ] Admin panel is accessible and functional
- [ ] Categories are created and organized
- [ ] Database migrations are applied
- [ ] All environment variables are set correctly

### Content Setup
- [ ] Welcome thread is created and pinned
- [ ] Forum guidelines are posted
- [ ] Initial categories are set up
- [ ] Test threads and posts are created
- [ ] Admin account has proper permissions

### User Experience
- [ ] Navigation is intuitive
- [ ] Registration process is smooth
- [ ] Thread creation and posting work
- [ ] Search functionality works
- [ ] Mobile responsiveness is good
- [ ] Loading times are acceptable

## 🔗 Next Steps

Your forum is now ready! Here's what to do next:

### For Development
- **[Configuration](../configuration/environment)** - Fine-tune your forum settings
- **[Customization](../customization/theming)** - Customize the appearance and branding
- **[Development Guide](../development/architecture)** - Learn about the codebase

### For Production
- **[Deployment Guide](../deployment/overview)** - Deploy your forum to production
- **[Production Checklist](../deployment/production-checklist)** - Pre-launch verification
- **[Admin Guide](../administration/admin-panel)** - Learn advanced administration

### For Growth
- **[User Guide](../guides/user-guide)** - Share with your users
- **[Moderation Guide](../guides/moderator-guide)** - Train your moderators
- **[Community Building](../guides/community-building)** - Grow your community

---

**Congratulations!** 🎉 Your OpenForum is now set up and ready for users. Happy community building!
