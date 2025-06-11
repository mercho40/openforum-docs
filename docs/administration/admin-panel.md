---
id: admin-panel
title: Admin Panel Guide
sidebar_label: Admin Panel
---

# Admin Panel Guide

The OpenForum admin panel provides comprehensive tools for managing your forum, users, content, and community. This guide covers all administrative features and best practices.

## 🎯 Accessing the Admin Panel

### Requirements
- User account with **admin** role
- Active login session
- Admin privileges enabled in database

### Access URL
Navigate to `/forum/admin` from your forum's base URL:
- Development: `http://localhost:3000/forum/admin`
- Production: `https://yourdomain.com/forum/admin`

:::info First Admin Setup
The first registered user must be manually promoted to admin via database update:
```sql
UPDATE "user" SET role = 'admin' WHERE email = 'your-email@example.com';
```
:::

## 🏠 Dashboard Overview

The admin dashboard provides a comprehensive overview of your forum's activity and health.

### Key Metrics

**Forum Statistics**
- Total threads and posts
- Active users (daily/weekly/monthly)
- New registrations
- Content growth trends

**User Engagement**
- Most active users
- Recent user registrations
- User retention metrics
- Popular discussion topics

**Content Overview**
- Recent threads and posts
- Reported content requiring attention
- Trending discussions
- Category activity levels

### Quick Actions

**Content Management**
- Review reported content
- Moderate recent posts
- Pin important announcements
- Lock inappropriate threads

**User Management**
- Review new user accounts
- Handle user reports
- Assign moderator roles
- Manage banned users

## 👥 User Management

### User Overview

**User Listing**
- View all registered users
- Search and filter users
- Sort by registration date, activity, role
- Export user data (GDPR compliance)

**User Details**
- Complete user profiles
- Registration information
- Activity history
- Reputation scores
- Moderation history

### Role Management

**Available Roles**
- **Admin**: Full forum management access
- **Moderator**: Content moderation and user management
- **User**: Standard forum participation
- **Banned**: Restricted access

**Assigning Roles**
```typescript
// Update user role
UPDATE "user" SET role = 'moderator' WHERE id = 'user-id';
```

**Role Permissions**

| Permission | User | Moderator | Admin |
|------------|------|-----------|-------|
| View content | ✅ | ✅ | ✅ |
| Create posts | ✅ | ✅ | ✅ |
| Edit own posts | ✅ | ✅ | ✅ |
| Delete own posts | ✅ | ✅ | ✅ |
| Edit any post | ❌ | ✅ | ✅ |
| Delete any post | ❌ | ✅ | ✅ |
| Pin/lock threads | ❌ | ✅ | ✅ |
| Manage categories | ❌ | ❌ | ✅ |
| Manage users | ❌ | ⚠️ Limited | ✅ |
| Access admin panel | ❌ | ⚠️ Limited | ✅ |

### User Moderation

**Banning Users**
1. Navigate to user profile
2. Click "Ban User"
3. Set ban reason and duration
4. Confirm ban action

**Ban Options**
- **Temporary ban**: Set expiration date
- **Permanent ban**: No expiration
- **IP ban**: Block IP address
- **Silent ban**: User unaware of ban status

**Unban Process**
1. Go to banned users list
2. Select user to unban
3. Remove ban reason
4. Restore user permissions

## 📁 Category Management

### Creating Categories

**Basic Category Setup**
1. **Navigate to Categories** in admin panel
2. **Click "New Category"**
3. **Fill out category details**:
   - **Name**: Descriptive category name
   - **Description**: Purpose and guidelines
   - **Slug**: URL-friendly identifier
   - **Icon**: Choose from Lucide icon library
   - **Color**: Brand color or theme color

**Advanced Settings**
- **Display Order**: Numerical sort order
- **Visibility**: Public or hidden categories
- **Permissions**: Role-based access control
- **Moderation**: Auto-moderation settings

### Category Organization

**Best Practices**
- **Logical grouping**: Related topics together
- **Clear naming**: Descriptive, unambiguous names
- **Appropriate icons**: Visual recognition aids
- **Consistent colors**: Brand-aligned color scheme
- **Proper ordering**: Most important categories first

**Example Category Structure**
```
📢 Announcements (pinned, admin-only posting)
💬 General Discussion (open to all users)
❓ Help & Support (help-focused discussions)
💡 Feature Requests (improvement suggestions)
🐛 Bug Reports (issue tracking)
👋 Introductions (new user welcomes)
```

### Category Moderation

**Thread Management**
- Pin important threads to top
- Lock threads to prevent replies
- Hide threads from public view
- Move threads between categories
- Merge duplicate threads

**Category Settings**
- **Read permissions**: Who can view
- **Write permissions**: Who can post
- **Moderation level**: Auto-approve vs review
- **Notification settings**: Admin alerts

## 🧵 Thread Moderation

### Thread Overview

**Thread Listing**
- View all forum threads
- Filter by category, author, status
- Sort by date, popularity, reports
- Bulk actions for multiple threads

**Thread Status**
- **Active**: Normal discussion state
- **Pinned**: Highlighted at top of category
- **Locked**: No new replies allowed
- **Hidden**: Invisible to regular users
- **Deleted**: Marked for removal

### Moderation Actions

**Individual Thread Actions**
1. **Pin Thread**: Highlight important discussions
   ```typescript
   UPDATE thread SET isPinned = true WHERE id = 'thread-id';
   ```

2. **Lock Thread**: Prevent new replies
   ```typescript
   UPDATE thread SET isLocked = true WHERE id = 'thread-id';
   ```

3. **Hide Thread**: Remove from public view
   ```typescript
   UPDATE thread SET isHidden = true WHERE id = 'thread-id';
   ```

4. **Move Thread**: Change category
   ```typescript
   UPDATE thread SET categoryId = 'new-category-id' WHERE id = 'thread-id';
   ```

**Bulk Actions**
- Select multiple threads
- Apply actions to all selected
- Export thread data
- Archive old discussions

### Content Guidelines

**Moderation Criteria**
- **Spam**: Promotional or irrelevant content
- **Harassment**: Personal attacks or bullying
- **Off-topic**: Content not relevant to category
- **Duplicate**: Redundant discussions
- **Inappropriate**: Offensive or harmful content

**Moderation Response**
1. **Review reported content**
2. **Assess violation severity**
3. **Apply appropriate action**
4. **Document moderation decision**
5. **Notify user if necessary**

## 📊 Reports Management

### Report Types

**Content Reports**
- Spam or promotional content
- Harassment or abuse
- Inappropriate content
- Copyright violations
- Misinformation

**User Reports**
- Harassment or stalking
- Fake accounts
- Ban evasion
- Terms of service violations

### Report Workflow

**Report Processing**
1. **Review report details**
   - Reported content or user
   - Reporter information
   - Report reason and details
   - Timestamp and context

2. **Investigate thoroughly**
   - Review reported content/user history
   - Check community guidelines
   - Consider context and severity
   - Gather additional evidence if needed

3. **Take appropriate action**
   - No action needed (false report)
   - Edit or remove content
   - Warn or suspend user
   - Ban user (temporary or permanent)

4. **Document decision**
   - Record action taken
   - Note reasoning
   - Update user/content status
   - Notify parties if appropriate

**Report Status Tracking**
- **Open**: Awaiting review
- **In Progress**: Under investigation
- **Resolved**: Action completed
- **Dismissed**: No action needed

## 📈 Analytics and Insights

### Forum Statistics

**Growth Metrics**
- New user registrations over time
- Thread and post creation trends
- Category activity levels
- User engagement patterns

**Popular Content**
- Most viewed threads
- Highest voted posts
- Most active categories
- Trending discussions

**User Behavior**
- Login frequency
- Posting patterns
- Category preferences
- Search queries

### Performance Monitoring

**System Health**
- Database query performance
- Page load times
- Error rates
- Uptime statistics

**Content Quality**
- Report-to-content ratio
- Moderation action frequency
- User satisfaction metrics
- Community growth health

## ⚙️ System Settings

### General Settings

**Forum Configuration**
```env
# Basic settings
APP_NAME="Your Forum Name"
FORUM_DESCRIPTION="Community discussion platform"
FORUM_URL="https://yourdomain.com"

# Registration settings
ALLOW_REGISTRATION="true"
REQUIRE_EMAIL_VERIFICATION="true"
DEFAULT_USER_ROLE="user"
```

**Email Settings**
- SMTP configuration
- Email templates
- Notification preferences
- Delivery monitoring

### Security Settings

**Authentication**
- Password requirements
- Two-factor authentication
- Session timeout
- Login attempt limits

**Content Security**
- File upload restrictions
- Content filtering
- Spam detection
- Rate limiting

## 🛠️ Maintenance Tasks

### Regular Maintenance

**Daily Tasks**
- [ ] Review reported content
- [ ] Check new user registrations
- [ ] Monitor system health
- [ ] Respond to community issues

**Weekly Tasks**
- [ ] Analyze forum statistics
- [ ] Update pinned announcements
- [ ] Review category organization
- [ ] Plan community events

**Monthly Tasks**
- [ ] Database maintenance
- [ ] Security updates
- [ ] Backup verification
- [ ] Performance optimization

### Database Maintenance

**Cleanup Operations**
```sql
-- Remove old deleted posts
DELETE FROM post WHERE isDeleted = true AND updatedAt < NOW() - INTERVAL '30 days';

-- Clean up expired sessions
DELETE FROM session WHERE expiresAt < NOW();

-- Archive old notifications
DELETE FROM notification WHERE createdAt < NOW() - INTERVAL '90 days' AND read = true;
```

**Performance Optimization**
- Rebuild database indexes
- Update query statistics
- Monitor slow queries
- Optimize database configuration

## 🚨 Emergency Procedures

### Security Incidents

**Suspected Account Compromise**
1. Immediately ban affected account
2. Review recent activity
3. Check for data breaches
4. Reset affected passwords
5. Notify users if necessary

**Content Spam Attack**
1. Identify spam pattern
2. Ban offending accounts
3. Remove spam content
4. Update spam filters
5. Monitor for continued attacks

**System Outages**
1. Identify root cause
2. Implement temporary fixes
3. Communicate with users
4. Apply permanent solution
5. Post-incident review

### Data Recovery

**Database Backup Restoration**
```bash
# Restore from backup
pg_restore --clean --no-acl --no-owner -h host -U user -d database backup_file.dump

# Verify data integrity
psql -h host -U user -d database -c "SELECT COUNT(*) FROM user;"
```

**Content Recovery**
- Restore deleted content from backups
- Recover user data from exports
- Rebuild search indexes
- Verify system functionality

This comprehensive admin panel guide enables effective forum management and community growth. Regular use of these tools ensures a healthy, engaged community environment.
