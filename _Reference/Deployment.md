# Deployment

Deployment is the process of making your web application available to users. It involves building, hosting, and maintaining your application on production servers.

## Deployment Process

### 1. Build Preparation

```bash
# Install dependencies
npm install

# Run tests
npm test

# Build for production
npm run build

# Lint code
npm run lint
```

### 2. Environment Configuration

```javascript
// .env.production
API_URL=https://api.production.com
NODE_ENV=production
DB_HOST=prod-db.example.com
SECRET_KEY=your-secret-key

// .env.staging
API_URL=https://api.staging.com
NODE_ENV=staging
```

```javascript
// Access environment variables
const apiUrl = process.env.API_URL;
const isProduction = process.env.NODE_ENV === 'production';
```

## Hosting Options

### Static Hosting (React, Vue, etc.)

```bash
# Deploy to Vercel
npx vercel

# Deploy to Netlify
npx netlify deploy --prod

# Deploy to GitHub Pages
npm run deploy
```

### Node.js Deployment

```bash
# Using PM2 for process management
pm2 start app.js --name myapp
pm2 save
pm2 startup

# Using Docker
docker build -t myapp .
docker run -p 3000:3000 myapp
```

## Deployment Platforms

### Vercel

```json
// vercel.json
{
  "version": 2,
  "builds": [
    {
      "src": "package.json",
      "use": "@vercel/node"
    }
  ],
  "routes": [
    {
      "src": "/(.*)",
      "dest": "/"
    }
  ]
}
```

### Netlify

```toml
# netlify.toml
[build]
  command = "npm run build"
  publish = "dist"

[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200
```

### AWS S3 + CloudFront

```javascript
// deploy.js
const AWS = require('aws-sdk');
const fs = require('fs');
const path = require('path');

const s3 = new AWS.S3();

async function deploy() {
  const buildDir = './build';
  const files = fs.readdirSync(buildDir);

  for (const file of files) {
    const filePath = path.join(buildDir, file);
    const content = fs.readFileSync(filePath);

    await s3.upload({
      Bucket: 'my-bucket',
      Key: file,
      Body: content,
      ContentType: getContentType(file)
    }).promise();
  }
}
```

## CI/CD Pipeline

### GitHub Actions

```yaml
# .github/workflows/deploy.yml
name: Deploy

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      
      - name: Setup Node.js
        uses: actions/setup-node@v2
        with:
          node-version: '16'
          
      - name: Install dependencies
        run: npm install
        
      - name: Run tests
        run: npm test
        
      - name: Build
        run: npm run build
        env:
          REACT_APP_API_URL: ${{ secrets.API_URL }}
          
      - name: Deploy
        run: npm run deploy
        env:
          DEPLOY_TOKEN: ${{ secrets.DEPLOY_TOKEN }}
```

## Docker Deployment

```dockerfile
# Dockerfile
FROM node:16-alpine

WORKDIR /app

COPY package*.json ./
RUN npm ci --only=production

COPY . .
RUN npm run build

EXPOSE 3000

CMD ["npm", "start"]
```

```yaml
# docker-compose.yml
version: '3.8'
services:
  web:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
      - DB_HOST=db
    depends_on:
      - db
      
  db:
    image: postgres:14
    environment:
      - POSTGRES_DB=myapp
      - POSTGRES_PASSWORD=secret
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```

## Domain and SSL

```nginx
# Nginx configuration
server {
    listen 80;
    server_name example.com;
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl;
    server_name example.com;

    ssl_certificate /path/to/cert.pem;
    ssl_certificate_key /path/to/key.pem;

    location / {
        root /var/www/html;
        try_files $uri $uri/ /index.html;
    }
}
```

## Common Use Cases

- Web applications
- Mobile app backends
- APIs and microservices
- Static websites
- E-commerce platforms

## Common Mistakes

1. **Not testing in production-like environment** - Test before deploying
2. **Hardcoding secrets** - Use environment variables
3. **Skipping error monitoring** - Set up logging and alerts
4. **No rollback plan** - Have a way to revert changes
5. **Ignoring performance** - Monitor and optimize post-deployment

## Related Topics

- [[CI-CD]]
- [[Docker]]
- [[AWS]]
- [[Vercel]]
- [[Netlify]]

## Quick Revision

| Step | Tools |
|------|-------|
| Build | npm run build |
| Test | npm test |
| Deploy | Vercel, Netlify, AWS |
| Monitor | Sentry, LogRocket |
| Scale | Load balancers, CDN |

Successful deployment requires careful planning, testing, and monitoring to ensure reliability and performance.