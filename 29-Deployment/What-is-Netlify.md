# What is Netlify?

## Definition

Netlify is a **web development platform** for deploying static sites, serverless functions, and full-stack applications with continuous deployment from Git.

## Key Features

| Feature | Description |
|---------|-------------|
| **Continuous Deployment** | Auto-deploy on Git push |
| **Serverless Functions** | Backend code without servers |
| **Form Handling** | Built-in form processing |
| **Identity** | User authentication |
| **Split Testing** | A/B testing |
| **Edge Handlers** | Custom server logic |

## Deployment Methods

```bash
# Method 1: Netlify CLI
npm install -g netlify-cli
netlify init
netlify deploy --prod

# Method 2: Git Integration
# Connect repo in Netlify dashboard
# Auto-deploys on push

# Method 3: Drag and drop
# Drag build folder to Netlify dashboard
```

## Netlify Configuration

```toml
# netlify.toml
[build]
  command = "npm run build"
  publish = "dist"

[build.environment]
  NODE_VERSION = "18"
  API_KEY = "secret"

[[redirects]]
  from = "/api/*"
  to = "/.netlify/functions/:splat"
  status = 200

[[headers]]
  for = "/*"
  [headers.values]
    X-Frame-Options = "DENY"
    X-XSS-Protection = "1; mode=block"
```

## Serverless Functions

```javascript
// netlify/functions/hello.js
exports.handler = async (event, context) => {
  return {
    statusCode: 200,
    body: JSON.stringify({
      message: "Hello from Netlify Functions!",
      path: event.path,
    }),
  };
};

// netlify/functions/users.js
const users = [
  { id: 1, name: "John" },
  { id: 2, name: "Jane" },
];

exports.handler = async (event) => {
  if (event.httpMethod === "GET") {
    return {
      statusCode: 200,
      body: JSON.stringify(users),
    };
  }

  if (event.httpMethod === "POST") {
    const data = JSON.parse(event.body);
    users.push(data);
    return {
      statusCode: 201,
      body: JSON.stringify(data),
    };
  }
};
```

## Environment Variables

```bash
# Set via Netlify UI or CLI
netlify env:set API_KEY "your-api-key"

# Access in functions
const apiKey = process.env.API_KEY;

# Access in frontend (must prefix with PUBLIC_)
const apiUrl = process.env.PUBLIC_API_URL;
```

## Redirects and Rewrites

```toml
# Simple redirect
[[redirects]]
  from = "/old-page"
  to = "/new-page"
  status = 301

# SPA fallback
[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200

# Proxy to API
[[redirects]]
  from = "/api/*"
  to = "https://api.example.com/:splat"
  status = 200
```

## Common Mistakes

```javascript
// BAD: Using process.env in frontend without PUBLIC_ prefix
const apiKey = process.env.API_KEY; // Won't work in browser

// GOOD: Use PUBLIC_ prefix for client-side variables
const apiUrl = process.env.PUBLIC_API_URL;

// BAD: Not handling errors in functions
exports.handler = async () => {
  const data = await fetchAPI(); // No error handling
  return { statusCode: 200, body: JSON.stringify(data) };
};

// GOOD: Always handle errors
exports.handler = async () => {
  try {
    const data = await fetchAPI();
    return { statusCode: 200, body: JSON.stringify(data) };
  } catch (error) {
    return {
      statusCode: 500,
      body: JSON.stringify({ error: error.message }),
    };
  }
};
```

## Free Tier Limits

| Feature | Free Tier |
|---------|-----------|
| Bandwidth | 100GB/month |
| Build minutes | 300 minutes |
| Serverless Functions | 125K requests |
| Forms | 100 submissions |
| Identity | 1,000 active users |

## Quick Revision

- Netlify hosts static sites and serverless functions
- Deploy via Git integration, CLI, or drag-and-drop
- Use `netlify.toml` for configuration
- Serverless functions go in `netlify/functions/`
- Environment variables need `PUBLIC_` prefix for client-side
- Free tier includes 100GB bandwidth

---

## Related Topics

- [[What-is-Deployment]] - Deployment overview
- [[What-is-CICD]] - CI/CD pipelines
- [[What-is-Vercel]] - Alternative platform
- [[Deploy-Netlify]] - Deploying to Netlify
- [[What-is-GitActions]] - GitHub Actions
- [[What-is-Docker]] - Containerization