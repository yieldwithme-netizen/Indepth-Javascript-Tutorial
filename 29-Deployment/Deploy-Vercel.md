# Deploy to Vercel

## Definition

Vercel is a **platform for frontend frameworks and static sites** with serverless functions, edge functions, and automatic CI/CD from Git.

## Deployment Methods

| Method | Description |
|--------|-------------|
| Git Integration | Auto-deploy on push |
| CLI | Manual deploy via command line |
| Dashboard | Import from Git |
| API | Programmatic deployment |

## Code Examples

### 1. Project Setup

```bash
# Initialize project
npm create vite@latest my-app -- --template react

# Install Vercel CLI
npm install -g vercel

# Login to Vercel
vercel login

# Initialize Vercel
vercel
```

### 2. Vercel Configuration File

```json
// vercel.json
{
  "buildCommand": "npm run build",
  "outputDirectory": "dist",
  "framework": "vite",
  "rewrites": [
    { "source": "/api/(.*)", "destination": "/api/$1" }
  ],
  "headers": [
    {
      "source": "/(.*)",
      "headers": [
        { "key": "X-Frame-Options", "value": "DENY" },
        { "key": "X-XSS-Protection", "value": "1; mode=block" }
      ]
    }
  ],
  "env": {
    "API_KEY": "@api-key"
  }
}
```

### 3. React + Vite Deployment

```javascript
// vite.config.js
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';

export default defineConfig({
  plugins: [react()],
  build: {
    outDir: 'dist'
  }
});

// package.json
{
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview"
  }
}
```

```bash
# Deploy
vercel

# Deploy to production
vercel --prod

# Deploy with specific name
vercel --name my-app
```

### 4. Next.js Deployment

```javascript
// next.config.js
module.exports = {
  output: 'standalone',
  images: {
    remotePatterns: [
      {
        protocol: 'https',
        hostname: 'example.com'
      }
    ]
  }
};
```

```bash
# Next.js auto-detected by Vercel
vercel

# With specific environment
vercel --environment production
```

### 5. Serverless API Routes

```javascript
// api/hello.js (Vercel Serverless Function)
export default function handler(req, res) {
  res.status(200).json({
    message: 'Hello from Vercel!',
    method: req.method,
    query: req.query
  });
}

// api/users/[id].js (Dynamic Route)
export default async function handler(req, res) {
  const { id } = req.query;

  const response = await fetch(`https://api.example.com/users/${id}`);
  const user = await response.json();

  res.status(200).json(user);
}

// api/submit.js (POST handler)
export default async function handler(req, res) {
  if (req.method !== 'POST') {
    return res.status(405).json({ error: 'Method not allowed' });
  }

  const { name, email } = req.body;

  // Process data
  await saveToDatabase({ name, email });

  res.status(201).json({ success: true });
}
```

### 6. Edge Functions

```javascript
// edge-functions/geolocation.js
export const config = {
  matcher: '/api/geo/:path*'
};

export default async function handler(request) {
  const url = new URL(request.url);
  const country = request.geo?.country || 'Unknown';

  return new Response(
    JSON.stringify({
      country,
      city: request.geo?.city,
      latitude: request.geo?.latitude
    }),
    {
      headers: { 'Content-Type': 'application/json' }
    }
  );
}
```

### 7. Environment Variables

```bash
# Set via CLI
vercel env add API_KEY production
vercel env add DATABASE_URL preview

# Pull environment variables
vercel env pull .env.local
```

```javascript
// Access in serverless functions
export default function handler(req, res) {
  const apiKey = process.env.API_KEY;
  const dbUrl = process.env.DATABASE_URL;

  // Use variables
  res.json({ apiKey: apiKey.substring(0, 4) + '...' });
}

// Access in client (must prefix with NEXT_PUBLIC_ for Next.js)
// NEXT_PUBLIC_API_KEY is exposed to browser
// API_KEY is only available server-side
```

### 8. Custom Domain Setup

```bash
# Add domain
vercel domains add example.com

# List domains
vercel domains ls

# Verify DNS records
# Add CNAME or A record pointing to Vercel
```

```javascript
// Domain configuration in vercel.json
{
  "domains": [
    "example.com",
    "www.example.com"
  ],
  "redirects": [
    {
      "source": "/:path",
      "has": [{ "type": "host", "value": "www.example.com" }],
      "destination": "https://example.com/:path",
      "permanent": true
    }
  ]
}
```

## Deployment Steps

```bash
# 1. Install Vercel CLI
npm i -g vercel

# 2. Login
vercel login

# 3. Test locally
vercel dev

# 4. Deploy preview
vercel

# 5. Deploy production
vercel --prod

# 6. Check deployment
vercel ls
```

## Common Use Cases

```javascript
// Monorepo configuration
// vercel.json
{
  "buildCommand": "turbo build --filter=@my-app/web",
  "outputDirectory": "apps/web/dist",
  "installCommand": "pnpm install"
}

// Preview deployments (automatic on PRs)
// Each PR gets unique URL for testing
// https://my-app-git-branch.vercel.app

// Rollback to previous deployment
vercel rollback
```

## Common Mistakes

| Mistake | Risk |
|---------|------|
| Wrong output directory | Build failures |
| Exposing secrets in client | Security risk |
| Not setting Node version | Build errors |
| Ignoring vercel.json | Misconfiguration |
| Not testing locally | Broken deployment |
| Missing API routes | 404 errors |
| Not configuring rewrites | SPA routing broken |

## Quick Revision

- Vercel auto-detects frameworks (React, Next.js, Vue)
- Use `vercel.json` for configuration
- Serverless functions go in `api/` directory
- Environment variables set via CLI or dashboard
- Preview deployments for every PR
- Custom domains require DNS configuration
- Edge functions for low-latency operations
- Use `vercel dev` for local development
- Automatic CI/CD from Git push

---

## Related Topics

- [[What-is-Vercel]] - Vercel overview
- [[Deploy-Netlify]] - Netlify deployment
- [[Setup-CICD]] - CI/CD configuration
- [[What-is-Deployment]] - Deployment concepts
- [[What-is-Docker]] - Container deployment
- [[What-is-GitActions]] - GitHub Actions
