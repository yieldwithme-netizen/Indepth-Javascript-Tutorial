# Deploy to Netlify

## Definition

Netlify is a **platform for deploying static sites and serverless functions** with features like continuous deployment, form handling, and edge functions.

## Deployment Methods

| Method | Description |
|--------|-------------|
| Git Integration | Auto-deploy on push |
| CLI | Manual deploy via command line |
| Drag & Drop | Upload folder in dashboard |
| API | Programmatic deployment |

## Code Examples

### 1. Project Setup

```bash
# Initialize project
mkdir my-site && cd my-site
npm init -y

# Install Netlify CLI
npm install -g netlify-cli

# Login to Netlify
netlify login

# Initialize Netlify
netlify init
```

### 2. Netlify Configuration File

```toml
# netlify.toml
[build]
  command = "npm run build"
  publish = "dist"

[build.environment]
  NODE_VERSION = "18"
  npm_config_production = "false"

# Redirects
[[redirects]]
  from = "/api/*"
  to = "/.netlify/functions/:splat"
  status = 200

# Headers
[[headers]]
  for = "/*"
  [headers.values]
    X-Frame-Options = "DENY"
    X-XSS-Protection = "1; mode=block"
    Content-Security-Policy = "default-src 'self'"
```

### 3. React App Deployment

```javascript
// package.json
{
  "name": "my-react-app",
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview"
  },
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0"
  },
  "devDependencies": {
    "vite": "^4.4.0"
  }
}

// vite.config.js
export default {
  build: {
    outDir: 'dist'
  }
}
```

```bash
# Deploy to Netlify
netlify deploy

# Production deploy
netlify deploy --prod

# Deploy specific directory
netlify deploy --dir=dist
```

### 4. Serverless Functions

```javascript
// netlify/functions/hello.js
exports.handler = async (event, context) => {
  return {
    statusCode: 200,
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      message: 'Hello from Netlify Functions!',
      path: event.path
    })
  };
};

// netlify/functions/users.js
exports.handler = async (event, context) => {
  const { httpMethod, body, queryStringParameters } = event;

  switch (httpMethod) {
    case 'GET':
      return {
        statusCode: 200,
        body: JSON.stringify({ users: ['Alice', 'Bob'] })
      };

    case 'POST':
      const newUser = JSON.parse(body);
      return {
        statusCode: 201,
        body: JSON.stringify(newUser)
      };

    default:
      return {
        statusCode: 405,
        body: JSON.stringify({ error: 'Method not allowed' })
      };
  }
};
```

### 5. Environment Variables

```bash
# Set environment variables
netlify env:set API_KEY "your-api-key"
netlify env:set DATABASE_URL "postgresql://..."

# Set for specific context
netlify env:set API_KEY "staging-key" --context deploy-preview
netlify env:set API_KEY "prod-key" --context production
```

```javascript
// Access environment variables in functions
exports.handler = async (event) => {
  const apiKey = process.env.API_KEY;
  const dbUrl = process.env.DATABASE_URL;

  // Use variables
  const data = await fetch(`https://api.example.com?key=${apiKey}`);

  return {
    statusCode: 200,
    body: JSON.stringify({ data: await data.json() })
  };
};
```

### 6. Form Handling

```html
<!-- Simple form -->
<form name="contact" method="POST" data-netlify="true">
  <input type="hidden" name="form-name" value="contact">
  <input type="text" name="name" placeholder="Name" required>
  <input type="email" name="email" placeholder="Email" required>
  <textarea name="message" placeholder="Message" required></textarea>
  <button type="submit">Send</button>
</form>
```

```javascript
// Access form submissions via API
const response = await fetch(
  `https://api.netlify.com/api/v1/sites/${siteId}/forms`,
  {
    headers: {
      'Authorization': `Bearer ${netlifyToken}`
    }
  }
);
const forms = await response.json();
```

### 7. Redirects and Rewrites

```toml
# netlify.toml

# Redirect old URLs
[[redirects]]
  from = "/old-page"
  to = "/new-page"
  status = 301

# SPA fallback
[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200

# Proxy to external API
[[redirects]]
  from = "/api/external/*"
  to = "https://api.external.com/:splat"
  status = 200
  force = true
```

### 8. Custom Domain Setup

```bash
# Add custom domain
netlify domains:add example.com

# Check DNS status
netlify domains:list

# Enable HTTPS (automatic with Let's Encrypt)
netlify https:setup
```

## Deployment Steps

```bash
# 1. Build your project
npm run build

# 2. Test locally
netlify dev

# 3. Deploy to preview
netlify deploy

# 4. Deploy to production
netlify deploy --prod

# 5. Verify deployment
open https://your-site.netlify.app
```

## Common Use Cases

```javascript
// Static site generator
// next.config.js for Netlify
module.exports = {
  output: 'export',  // Static export
  distDir: 'dist'
};

// Gatsby on Netlify
// gatsby-config.js
module.exports = {
  plugins: [
    'gatsby-plugin-netlify-cms',
    'gatsby-plugin-netlify'
  ]
};
```

## Common Mistakes

| Mistake | Risk |
|---------|------|
| Wrong publish directory | 404 errors |
| Missing build command | No deployment |
| Not setting NODE_VERSION | Build failures |
| Ignoring netlify.toml | Misconfiguration |
| Not testing locally | Broken production |
| Exposing secrets in frontend | Security risk |
| Missing SPA redirects | Routing issues |

## Quick Revision

- Use `netlify.toml` for configuration
- Set correct `publish` directory for built files
- Use Netlify Functions for serverless backend
- Set environment variables via CLI or dashboard
- Configure redirects for SPA routing
- Test locally with `netlify dev` before deploying
- Custom domains require DNS configuration
- HTTPS is automatic with Let's Encrypt
- Use form handling for contact forms

---

## Related Topics

- [[What-is-Netlify]] - Netlify overview
- [[Deploy-Vercel]] - Vercel deployment
- [[Setup-CICD]] - CI/CD configuration
- [[What-is-Deployment]] - Deployment concepts
- [[What-is-Docker]] - Container deployment
- [[What-is-GitActions]] - GitHub Actions
