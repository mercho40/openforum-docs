---
id: project-structure
title: Project Structure
sidebar_label: Project Structure
---

# Project Structure

Understanding OpenForum's project structure will help you navigate the codebase, customize features, and contribute effectively. This guide provides a comprehensive overview of how the project is organized.

## 📂 High-Level Structure

```
openforum/
├── 📁 app/                    # Next.js 15 App Router - Main application pages
├── 📁 components/             # React components - Reusable UI components  
├── 📁 db/                     # Database - Schema and connection configuration
├── 📁 lib/                    # Libraries - Utility functions and configurations
├── 📁 actions/                # Server Actions - Backend logic and API operations
├── 📁 migrations/             # Database Migrations - Schema version control
├── 📁 docs/                   # Documentation - Guides and API references
├── 📁 public/                 # Static Assets - Images, icons, and public files
├── 📄 package.json            # Dependencies and scripts
├── 📄 next.config.ts          # Next.js configuration
├── 📄 tailwind.config.ts      # Tailwind CSS configuration
├── 📄 drizzle.config.ts       # Database ORM configuration
└── 📄 tsconfig.json           # TypeScript configuration
```

## 🗂️ App Directory (`/app`)

The `app` directory uses Next.js 15's App Router for modern React server components.

### Main Application Structure

```
app/
├── layout.tsx                 # Root layout - Global app wrapper
├── page.tsx                   # Landing page - Forum homepage
├── globals.css                # Global styles - Tailwind and custom CSS
├── favicon.ico                # Site favicon
│
├── 🔐 auth/                   # Authentication pages
│   ├── signin/page.tsx        # Sign in form
│   ├── signup/page.tsx        # Registration form
│   ├── verify-email/page.tsx  # Email verification
│   ├── forgot-password/page.tsx # Password reset
│   ├── complete-profile/page.tsx # OAuth profile completion
│   └── callback/page.tsx      # OAuth callback handler
│
├── 🌐 api/                    # API routes
│   └── auth/[...all]/route.ts # Better Auth API handler
│
└── 💬 forum/                  # Main forum application
    ├── page.tsx               # Forum homepage
    ├── categories/            # Category pages
    ├── threads/               # Thread pages
    ├── members/               # Member pages
    ├── search/                # Search functionality
    ├── profile/               # User profiles
    └── admin/                 # Admin panel
```

### Forum Structure Deep Dive

```
forum/
├── page.tsx                   # Forum homepage with categories and recent threads
│
├── 📁 categories/
│   ├── page.tsx               # All categories overview
│   └── [categorySlug]/
│       ├── page.tsx           # Category thread listing
│       ├── new-thread/page.tsx # Create new thread in category
│       └── threads/[threadSlug]/
│           └── page.tsx       # Individual thread view
│
├── 📁 threads/
│   ├── page.tsx               # All threads view with filtering
│   └── new/page.tsx           # Create thread (choose category)
│
├── 📁 admin/                  # Admin panel (role-protected)
│   ├── layout.tsx             # Admin layout with navigation
│   ├── page.tsx               # Admin dashboard
│   ├── categories/            # Category management
│   ├── threads/               # Thread moderation
│   ├── users/                 # User management
│   ├── reports/               # Report handling
│   └── tags/                  # Tag management
│
├── 📁 members/
│   └── page.tsx               # Member directory
│
├── 📁 profile/
│   ├── page.tsx               # Current user profile
│   └── [userId]/page.tsx      # Public user profiles
│
└── 📁 search/
    └── page.tsx               # Search results
```

## 🧩 Components Directory (`/components`)

Organized by feature and reusability level.

```
components/
├── 🔧 ui/                     # Base UI components (shadcn/ui)
│   ├── button.tsx
│   ├── card.tsx
│   ├── dialog.tsx
│   ├── form.tsx
│   ├── input.tsx
│   └── ... (other UI primitives)
│
├── 🔐 auth/                   # Authentication components
│   ├── LoginForm.tsx          # Sign in form logic
│   ├── RegisterForm.tsx       # Registration form logic
│   ├── ForgotPassword.tsx     # Password reset form
│   └── CompleteProfileForm.tsx # OAuth profile completion
│
├── 💬 forum/                  # Forum-specific components
│   ├── CategoryIcon.tsx       # Category icon renderer
│   ├── forms/                 # Forum forms
│   │   ├── NewThreadForm.tsx  # Thread creation form
│   │   ├── NewPostForm.tsx    # Post creation form
│   │   └── EditPostForm.tsx   # Post editing form
│   └── admin/                 # Admin components
│       ├── CategoryEditForm.tsx
│       ├── IconPicker.tsx
│       ├── ColorPicker.tsx
│       └── MobileNav.tsx
│
├── 📧 email/                  # Email template components
│   └── verification-template.tsx
│
├── 📱 views/                  # Page-level view components
│   ├── auth/                  # Authentication views
│   ├── forum/                 # Forum views
│   │   ├── ForumHomeView.tsx  # Main forum homepage
│   │   ├── CategoryView.tsx   # Category thread listing
│   │   ├── ThreadView.tsx     # Thread discussion view
│   │   ├── AllThreadsView.tsx # All threads with filters
│   │   └── CategoriesView.tsx # Categories overview
│   └── admin/                 # Admin panel views
│
└── 🔧 shared/                 # Shared utility components
    ├── BackButton.tsx
    ├── ThreadCard.tsx
    └── VerifyEmail.tsx
```

## 🗄️ Database Directory (`/db`)

Database configuration and schema definition.

```
db/
├── drizzle.ts                 # Database connection and client setup
└── schema.ts                  # Complete database schema definition
```

### Schema Overview

The database schema includes these main entities:

- **Users**: Authentication, profiles, roles, reputation
- **Categories**: Forum organization with metadata
- **Threads**: Discussion topics with stats
- **Posts**: Thread replies with voting
- **Tags**: Thread categorization
- **Notifications**: User notification system
- **Subscriptions**: Thread/category following
- **Sessions**: Authentication sessions
- **Reports**: Content moderation

## 📚 Libraries Directory (`/lib`)

Utility functions and configuration files.

```
lib/
├── auth.ts                    # Better Auth server configuration
├── auth-client.ts             # Client-side auth utilities
└── utils.ts                   # General utility functions (cn, etc.)
```

## ⚡ Actions Directory (`/actions`)

Server Actions for backend operations (Next.js 15 Server Actions).

```
actions/
├── category.ts                # Category CRUD operations
├── thread.ts                  # Thread management
├── post.ts                    # Post operations and voting
├── user.ts                    # User management
├── tag.ts                     # Tag operations
├── stats.ts                   # Analytics and statistics
├── subscription.ts            # Subscription management
├── notification.ts            # Notification system
└── email.ts                   # Email sending utilities
```

### Action Examples

Each action file typically includes:
- **CRUD operations** (Create, Read, Update, Delete)
- **Business logic** (voting, reputation, etc.)
- **Authorization checks** (user permissions)
- **Database queries** with Drizzle ORM

## 🔄 Migrations Directory (`/migrations`)

Database version control and schema changes.

```
migrations/
├── 0000_abnormal_maddog.sql   # Initial schema
├── 0002_violet_big_bertha.sql # Schema updates
├── 0003_jittery_aqueduct.sql  # Latest changes
└── meta/                      # Migration metadata
    ├── _journal.json          # Migration history
    ├── 0000_snapshot.json     # Schema snapshots
    ├── 0001_snapshot.json
    └── ...
```

## 📖 Documentation Directory (`/docs`)

Project documentation and guides.

```
docs/
├── README.md                  # Documentation overview
├── getting-started.md         # Setup guide
├── configuration.md           # Configuration options
└── docusaurus/               # Docusaurus documentation site
    ├── docusaurus.config.js
    ├── sidebars.js
    └── docs/                 # Structured documentation
```

## 🎯 Key File Purposes

### Configuration Files

| File | Purpose |
|------|---------|
| `package.json` | Dependencies, scripts, project metadata |
| `next.config.ts` | Next.js configuration, image domains, etc. |
| `tailwind.config.ts` | Tailwind CSS customization |
| `drizzle.config.ts` | Database ORM configuration |
| `tsconfig.json` | TypeScript compiler settings |
| `eslint.config.mjs` | Code linting rules |
| `components.json` | shadcn/ui component configuration |

### Key Application Files

| File | Purpose |
|------|---------|
| `app/layout.tsx` | Root layout, providers, metadata |
| `app/globals.css` | Global styles, Tailwind imports |
| `lib/auth.ts` | Authentication configuration |
| `db/schema.ts` | Database schema definition |
| `components/views/forum/ForumHomeView.tsx` | Main forum interface |

## 🔍 Navigation Patterns

### File-based Routing

OpenForum uses Next.js App Router file-based routing:

- `app/forum/page.tsx` → `/forum`
- `app/forum/categories/page.tsx` → `/forum/categories`
- `app/forum/categories/[categorySlug]/page.tsx` → `/forum/categories/general`
- `app/forum/admin/page.tsx` → `/forum/admin`

### Dynamic Routes

- `[categorySlug]` - Category slug parameter
- `[threadSlug]` - Thread slug parameter  
- `[userId]` - User ID parameter
- `[...all]` - Catch-all route for auth

## 🎨 Styling Architecture

### Tailwind CSS

- **Global styles**: `app/globals.css`
- **Component styles**: Utility classes in components
- **Custom components**: `components/ui/` (shadcn/ui)
- **Theme configuration**: `tailwind.config.ts`

### CSS Variables

```css
/* Light mode */
:root {
  --background: 0 0% 100%;
  --foreground: 222.2 84% 4.9%;
  --primary: 221.2 83.2% 53.3%;
  /* ... */
}

/* Dark mode */
.dark {
  --background: 222.2 84% 4.9%;
  --foreground: 210 40% 98%;
  /* ... */
}
```

## 🚀 Build and Development

### Development Scripts

```bash
# Development server
pnpm dev

# Database operations
pnpm drizzle-kit generate    # Generate migrations
pnpm drizzle-kit migrate     # Apply migrations  
pnpm drizzle-kit studio      # Database GUI

# Code quality
pnpm lint                    # ESLint
pnpm type-check             # TypeScript

# Production
pnpm build                   # Build for production
pnpm start                   # Start production server
```

### Environment Files

- `.env.local` - Local development environment
- `.env.example` - Example environment variables
- `.env.production` - Production environment (if needed)

## 🔗 Understanding the Flow

### User Request Flow

1. **Route**: Next.js App Router matches URL to page component
2. **Layout**: Root layout provides global structure
3. **Page**: Page component handles the route
4. **View Component**: Large view components organize the page
5. **UI Components**: Smaller components handle specific functionality
6. **Server Actions**: Backend operations for data manipulation
7. **Database**: Drizzle ORM queries PostgreSQL

### Component Hierarchy

```
Page Component
└── View Component (e.g., ForumHomeView)
    ├── UI Components (Button, Card, etc.)
    ├── Form Components (NewThreadForm, etc.)
    └── Feature Components (CategoryIcon, etc.)
```

## 📝 Naming Conventions

### Files and Directories
- **PascalCase**: React components (`ForumHomeView.tsx`)
- **kebab-case**: Routes and static files (`forgot-password/`)
- **camelCase**: Utility functions and variables
- **lowercase**: Configuration files (`package.json`)

### React Components
- **Views**: `*View.tsx` (page-level components)
- **Forms**: `*Form.tsx` (form components)
- **UI**: Descriptive names (`Button.tsx`, `Card.tsx`)
- **Features**: Descriptive names (`CategoryIcon.tsx`)

This structure promotes maintainability, scalability, and developer experience. Each directory has a clear purpose, making it easy to locate and modify specific functionality.
