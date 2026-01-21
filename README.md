# SignalRoot Documentation

Mintlify-style documentation site for SignalRoot alert enrichment platform.

## 🚀 Quick Start

```bash
# Install dependencies
npm install

# Start development server
npm run dev

# Build for production
npm run build

# Serve locally
npm run serve
```

## 📁 Project Structure

```
signalroot-docs/
├── docs/                   # Documentation content
│   ├── overview.md       # Product overview
│   ├── quickstart.md     # Getting started guide
│   ├── api-reference.md  # API documentation
│   └── swagger.html       # Interactive Swagger UI
├── index.html             # Homepage with navigation
├── getting-started.html   # Detailed quick start guide
├── api-reference.html     # Complete API reference
├── mint.json             # Mintlify configuration
├── package.json          # Dependencies and scripts
└── README.md             # This file
```

## 🎨 Features

- **Mintlify Style**: Modern documentation framework
- **Tailwind CSS**: Utility-first CSS framework
- **Responsive Design**: Mobile-first approach
- **Interactive Navigation**: Sidebar and breadcrumb navigation
- **Code Highlighting**: Syntax highlighting for all code blocks
- **Search Ready**: Structure supports future search implementation
- **SEO Optimized**: Meta tags and semantic HTML

## 🛠️ Development

### Environment Setup

1. **Node.js**: Version 16 or higher
2. **npm**: Latest version
3. **Browser**: Chrome, Firefox, Safari, Edge

### Available Scripts

```bash
npm run dev          # Start development server
npm run build        # Build static site
npm run serve         # Serve built site locally
npm run lint          # Validate documentation structure
```

### Configuration

#### mint.json

```json
{
  "$schema": "https://mintlify.com/schema.json",
  "name": "SignalRoot Documentation",
  "colors": {
    "primary": "#6366f1",
    "light": "#818cf8",
    "dark": "#4f46e5"
  },
  "navigation": [
    {
      "group": "Getting Started",
      "pages": ["overview", "quickstart"]
    },
    {
      "group": "API Reference", 
      "pages": ["api-reference"]
    }
  ],
  "search": true,
  "feedback": {
    "thumbsRating": true,
    "suggestEdit": true
  }
}
```

## 📚 Content Creation

### Markdown Guidelines

- **Headings**: Use `#`, `##`, `###` for hierarchy
- **Code Blocks**: Use triple backticks with language identifier
- **Links**: Use `[text](url)` format
- **Lists**: Use `-` or `1.` for bullet points
- **Emphasis**: Use `**bold**` or `*italic*`
- **Tables**: Use pipe `|` syntax for alignment

### Front Matter

```yaml
---
title: "Page Title"
description: "Page description for SEO"
lastUpdated: "2024-01-20"
tags: ["api", "documentation"]
---
```

## 🚀 Deployment

### Build Process

```bash
# Create optimized build
npm run build

# Output in dist/
# Ready for static hosting
```

### Deployment Options

1. **Mintlify**: Recommended for documentation sites
2. **Vercel**: Static hosting with CI/CD
3. **Netlify**: Static hosting with form processing
4. **GitHub Pages**: Free static hosting
5. **AWS S3**: Static file hosting with CloudFront

### Environment Variables

```bash
# Production build
npm run build

# Deploy to Vercel
vercel --prod

# Custom domain
vercel --prod --domain docs.signalroot.com
```

### Custom Domain Setup

```json
{
  "customDomain": "docs.signalroot.com",
  "buildCommand": "npm run build",
  "outputDirectory": "dist",
  "installCommand": "npm install",
  "framework": null
}
```

## 🔗 Integration

### External Links

```html
<!-- Link to main app -->
<a href="https://app.signalroot.com" class="text-indigo-600 hover:text-indigo-700">
    🚀 Launch SignalRoot App
</a>

<!-- Link to backend API -->
<a href="https://api.signalroot.com" class="text-indigo-600 hover:text-indigo-700">
    🔌 API Documentation
</a>

<!-- Link to Swagger UI -->
<a href="https://api.signalroot.com/swagger-ui.html" class="text-indigo-600 hover:text-indigo-700">
    📚 Swagger UI
</a>
```

## 📊 Analytics

### Mintlify Analytics

Built-in analytics for documentation usage:

- **Page Views**: Track most visited documentation pages
- **Search Queries**: Understand what users are looking for
- **User Journey**: See how users navigate through documentation
- **Performance**: Monitor page load times and interactions

### Custom Analytics

```html
<!-- Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id=GA_MEASUREMENT_ID"></script>
```

## 🧪 Testing

### Content Validation

```bash
# Validate Mintlify configuration
npm run lint

# Check markdown syntax
npx markdownlint docs/**/*.md

# Test all links
npx linkcheck docs/
```

### Accessibility Testing

```bash
# Automated accessibility testing
npm install -g pa11y
pa11y docs/

# Manual testing checklist
- [ ] Keyboard navigation works
- [ ] Screen reader compatible
- [ ] Sufficient color contrast
- [ ] Proper heading hierarchy
```

## 🔧 Troubleshooting

### Common Issues

1. **Build Errors**: Check Node.js version and dependencies
2. **Navigation Issues**: Verify mint.json structure
3. **Styling Problems**: Check Tailwind CSS classes
4. **Link Errors**: Validate all internal and external links

### Debug Mode

```bash
# Enable verbose logging
DEBUG=true npm run dev

# Local development with HTTPS
npm run dev --https
```

## 📄 License

MIT License - see LICENSE file for details.

## 📞 Support

- **Documentation**: [SignalRoot Docs](https://docs.signalroot.com)
- **Mintlify Guide**: [Mintlify Documentation](https://mintlify.com/docs)
- **Issues**: [GitHub Issues](https://github.com/signalroot/signalroot-docs/issues)
- **Discord**: [Community Discord](https://discord.gg/signalroot)
- **Email**: [docs-support@signalroot.com](mailto:docs-support@signalroot.com)

---

Built with Mintlify, Tailwind CSS, and modern web standards.
