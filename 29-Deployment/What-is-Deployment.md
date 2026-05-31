# What is Deployment?

## Definition

Deployment is **making your application available** to users on a server.

## Deployment Platforms

| Platform | Type | Best For |
|----------|------|----------|
| Vercel | Frontend/Serverless | React, Next.js |
| Netlify | Frontend/Serverless | Static sites |
| AWS | Full cloud | Enterprise |
| Heroku | PaaS | Full-stack apps |
| DigitalOcean | VPS | Custom setups |

## Deployment Steps

```bash
# 1. Build
npm run build

# 2. Test
npm test

# 3. Commit to Git
git add .
git commit -m "Ready for deployment"
git push

# 4. Deploy (platform-specific)
# Vercel: vercel deploy
# Netlify: netlify deploy
# Heroku: git push heroku main
```

## Environment Variables

```javascript
// .env (never commit this!)
API_KEY=secret123
DATABASE_URL=postgresql://...

// Access in code
const apiKey = process.env.API_KEY;
```

## Quick Revision

- Deployment = making app available
- Platforms: Vercel, Netlify, AWS, Heroku
- Build → Test → Commit → Deploy
- Use environment variables for secrets
- Never commit .env files

---

## Related Topics

- [[What-is-Deployment]] - Deployment overview
- [[What-is-Vercel]] - Vercel
- [[What-is-Netlify]] - Netlify
- [[What-is-AWS]] - AWS
- [[Deploy-Vercel]] - Deploying to Vercel
- [[Deploy-Netlify]] - Deploying to Netlify
