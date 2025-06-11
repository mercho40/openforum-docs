# OpenForum Documentation

This directory contains the complete Docusaurus documentation site for OpenForum.

## 🚀 Quick Start

### Prerequisites
- Node.js 18 or higher
- pnpm (recommended) or npm

### Installation

```bash
# Navigate to the docs directory
cd docs/docusaurus

# Install dependencies
pnpm install

# Start development server
pnpm start
```

The documentation site will be available at `http://localhost:3000`.

### Building for Production

```bash
# Build static site
pnpm build

# Serve built site locally
pnpm serve
```

## 📚 Documentation Structure

### Getting Started
- **Overview** - Introduction to OpenForum
- **Installation** - Step-by-step setup guide
- **First Steps** - Post-installation configuration
- **Project Structure** - Codebase organization

### Configuration
- **Environment Variables** - All configuration options
- **Database Setup** - Database configuration
- **Authentication** - Auth providers and security
- **Email Configuration** - Email service setup
- **OAuth Setup** - Social login configuration
- **Application Settings** - General app configuration

### Customization
- **Theming** - UI customization and branding
- **Branding** - Logo, colors, and identity
- **Categories** - Forum category setup
- **Email Templates** - Custom email designs
- **Components** - UI component customization

### Deployment
- **Overview** - Deployment options comparison
- **Vercel** - Recommended deployment platform
- **Railway** - Alternative deployment option
- **Docker** - Container deployment
- **Self-hosted** - VPS/server deployment
- **Production Checklist** - Pre-launch verification

### Administration
- **Admin Panel** - Complete admin guide
- **User Management** - Managing forum users
- **Category Management** - Organizing discussions
- **Thread Moderation** - Content moderation
- **Reports** - Handling user reports
- **Analytics** - Forum statistics and insights

### Development
- **Contributing** - How to contribute code
- **Architecture** - Technical architecture overview
- **Database Schema** - Database structure
- **API Reference** - Complete API documentation
- **Components** - Component library
- **Testing** - Testing strategies

### Guides
- **User Guide** - End-user forum usage
- **Moderator Guide** - Moderation best practices
- **Migration** - Migrating from other platforms
- **Backup & Restore** - Data management
- **Troubleshooting** - Common issues and solutions

### Support
- **FAQ** - Frequently asked questions
- **Community** - Getting help and support
- **Issues** - Bug reporting and feature requests
- **Changelog** - Version history and updates

## 🎨 Customizing Documentation

### Updating Content

1. **Edit Markdown Files**: Modify files in `docs/` directory
2. **Update Sidebars**: Modify `sidebars.js` for navigation
3. **Add New Pages**: Create new `.md` files and update sidebars
4. **Update Config**: Modify `docusaurus.config.js` for site settings

### Styling

1. **Custom CSS**: Edit `src/css/custom.css`
2. **Theme Config**: Update theme settings in config file
3. **Component Swizzling**: Customize Docusaurus components

### Configuration

Key configuration files:
- `docusaurus.config.js` - Main site configuration
- `sidebars.js` - Navigation structure
- `package.json` - Dependencies and scripts

## 🚀 Deployment

### GitHub Pages

```bash
# Set up GitHub Pages deployment
GIT_USER=your-username pnpm deploy
```

### Vercel

1. Connect repository to Vercel
2. Set build command: `cd docs/docusaurus && pnpm build`
3. Set output directory: `docs/docusaurus/build`
4. Deploy automatically on git push

### Netlify

1. Connect repository to Netlify
2. Set build command: `cd docs/docusaurus && pnpm build`
3. Set publish directory: `docs/docusaurus/build`
4. Deploy automatically on git push

## 📝 Contributing to Documentation

### Writing Guidelines

1. **Clear and Concise**: Write in simple, accessible language
2. **Step-by-Step**: Break complex processes into steps
3. **Code Examples**: Include practical code samples
4. **Screenshots**: Add visuals where helpful
5. **Links**: Cross-reference related documentation

### Documentation Standards

1. **Frontmatter**: Include proper metadata in each file
2. **Headings**: Use proper heading hierarchy
3. **Code Blocks**: Include language specification
4. **Admonitions**: Use info, warning, tip callouts appropriately

### Review Process

1. **Local Testing**: Test documentation locally
2. **Link Checking**: Verify all links work
3. **Grammar Check**: Review for spelling and grammar
4. **Technical Review**: Ensure technical accuracy

## 🔧 Maintenance

### Regular Updates

1. **Keep Content Current**: Update for new features
2. **Fix Broken Links**: Regular link checking
3. **Update Screenshots**: Keep visuals current
4. **Version Updates**: Update for new releases

### Analytics

If you set up analytics:
- Monitor popular pages
- Identify areas needing improvement
- Track user feedback
- Update based on usage patterns

## 📞 Support

For documentation issues:
1. Check existing issues
2. Open new issue with documentation label
3. Provide specific page and section
4. Suggest improvements

---

**Happy documenting!** 📚 This comprehensive documentation helps users get the most out of OpenForum.
