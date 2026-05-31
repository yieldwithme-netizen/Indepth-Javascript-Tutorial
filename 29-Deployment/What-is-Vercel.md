# What is Vercel?

## Definition

Vercel is a **frontend cloud platform** for deploying web applications with zero configuration, automatic HTTPS, and edge network delivery.

## Key Features

| Feature | Description |
|---------|-------------|
| **Zero Config** | Auto-detects framework settings |
| **Edge Network** | Global CDN for fast loading |
| **Preview Deployments** | Every PR gets a preview URL |
| **Serverless Functions** | Backend logic at the edge |
| **Analytics** | Built-in performance monitoring |
| **Web Analytics** | Privacy-friendly tracking |

## Supported Frameworks

```bash
# Frameworks with zero config
Next.js    # Built by Vercel
React      # CRA or custom
Vue        # Vue CLI
Nuxt       # Nuxt.js
Angular    # Angular CLI
Svelte     # SvelteKit
Gatsby     # Static sites
```

## Deployment Methods

```bash
# Method 1: Vercel CLI
npm install -g vercel
vercel login
vercel

# Method 2: Git Integration
# Connect repo in Vercel dashboard

# Method 3: Vercel CLI with flags
vercel --prod --yes
```

## Vercel Configuration

```json
// vercel.json
{
  "version": 2,
  "builds": [
    {
      "src": "package.json",
      "use": "@vercel/static-build",
      "config": {
        "distDir": "dist"
      }
    }
  ],
  "routes": [
    { "src": "/api/(.*)", "dest": "/api/$1" },
    { "src": "/(.*)", "dest": "/index.html" }
  ],
  "env": {
    "API_KEY": "@api-key"
  }
}
```

## Serverless API Routes (Next.js)

```javascript
// pages/api/hello.js
export default function handler(req, res) {
  res.status(200).json({
    message: "Hello from Vercel!",
    method: req.method,
  });
}

// pages/api/users/[id].js
export default function handler(req, res) {
  const { id } = req.query;
  res.status(200).json({ id, name: "User" });
}

// app/api/hello/route.js (App Router)
export async function GET() {
  return Response.json({ message: "Hello" });
}

export async function POST(request) {
  const body = await request.json();
  return Response.json({ received: body }, { status: 201 });
}
```

## Environment Variables

```bash
# Set via Vercel UI or CLI
vercel env add API_KEY

# Access in code
const apiKey = process.env.API_KEY;

# Environment-specific
vercel env add DATABASE_URL production
vercel env add DATABASE_URL preview
vercel env add DATABASE_URL development
```

## Preview Deployments

```yaml
# Automatic preview deployments on PRs
# Every push to non-main branch gets a unique URL
# https://your-project-git-branch-name.vercel.app

# Configure in vercel.json
{
  "github": {
    "silent": false
  }
}
```

## Edge Middleware

```javascript
// middleware.js
import { NextResponse } from "next/server";

export function middleware(request) {
  // Run at the edge before page loads
  const token = request.cookies.get("token");

  if (!token) {
    return NextResponse.redirect(new URL("/login", request.url));
  }

  return NextResponse.next();
}

export const config = {
  matcher: ["/dashboard/:path*"],
};
```

## Common Mistakes

```javascript
// BAD: Using Node.js APIs in client components
"use client";
import fs from "fs"; // Won't work

// GOOD: Use server-side for Node.js APIs
// app/api/data/route.js
import fs from "fs";

export async function GET() {
  const data = fs.readFileSync("data.json");
  return Response.json(JSON.parse(data));
}

// BAD: Hardcoding API URLs
const apiUrl = "https://api.example.com";

// GOOD: Use environment variables
const apiUrl = process.env.NEXT_PUBLIC_API_URL;
```

## Vercel vs Netlify

| Feature | Vercel | Netlify |
|---------|--------|---------|
| Framework Support | Next.js (native) | All |
| Edge Functions | Yes | Yes |
| Preview Deployments | Yes | Yes |
| Analytics | Built-in | Third-party |
| Free Tier | 100GB bandwidth | 100GB bandwidth |
| Build Minutes | 6000/month | 300/month |

## Quick Revision

- Vercel is optimized for frontend frameworks
- Zero configuration for supported frameworks
- Every PR gets a preview deployment
- Serverless functions for API routes
- Edge middleware for request filtering
- Built-in analytics and monitoring

---

## Related Topics

- [[What-is-Deployment]] - Deployment overview
- [[What-is-Netlify]] - Alternative platform
- [[What-is-CICD]] - CI/CD pipelines
- [[What-is-GitActions]] - GitHub Actions
- [[Deploy-Vercel]] - Deploying to Vercel
- [[What-is-Express]] - Backend with Express