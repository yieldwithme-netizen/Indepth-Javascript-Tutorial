# What is CI/CD?

## Definition

CI/CD stands for **Continuous Integration / Continuous Deployment** — automated processes that build, test, and deploy code whenever changes are pushed to a repository.

## CI vs CD

| Concept | What It Does | Trigger |
|---------|--------------|---------|
| **CI** (Continuous Integration) | Automatically builds and tests code | Push to repository |
| **CD** (Continuous Delivery) | Automatically prepares release | After CI passes |
| **CD** (Continuous Deployment) | Automatically deploys to production | After tests pass |

## CI/CD Pipeline Flow

```
Code Push → Build → Test → Deploy (Staging) → Deploy (Production)
```

## GitHub Actions Example

```yaml
# .github/workflows/ci.yml
name: CI/CD Pipeline

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  build-and-test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'

      - name: Install dependencies
        run: npm ci

      - name: Run tests
        run: npm test

      - name: Build project
        run: npm run build

      - name: Deploy to production
        if: github.ref == 'refs/heads/main'
        run: npm run deploy
        env:
          DEPLOY_TOKEN: ${{ secrets.DEPLOY_TOKEN }}
```

## Common CI/CD Tools

| Tool | Type | Best For |
|------|------|----------|
| GitHub Actions | CI/CD | GitHub projects |
| Jenkins | CI/CD | Enterprise |
| GitLab CI | CI/CD | GitLab projects |
| CircleCI | CI/CD | Cloud-native |
| Travis CI | CI | Open source |

## Benefits of CI/CD

```javascript
// Manual process (slow, error-prone)
// 1. Pull latest code
// 2. Run tests manually
// 3. Build manually
// 4. Deploy manually

// Automated CI/CD (fast, reliable)
// Push code → Everything happens automatically
```

## Common Mistakes

```yaml
# BAD: Not caching dependencies
- run: npm install  # Downloads everything each time

# GOOD: Using cache
- uses: actions/cache@v3
  with:
    path: ~/.npm
    key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
- run: npm ci

# BAD: Deploying without tests
- run: npm run deploy

# GOOD: Test first, then deploy
- run: npm test
- run: npm run deploy
```

## Quick Revision

- CI = Continuous Integration (build + test on every push)
- CD = Continuous Delivery/Deployment (automated release)
- Popular tools: GitHub Actions, Jenkins, GitLab CI
- Always test before deploying
- Use caching to speed up pipelines
- Store secrets in environment variables, never in code

---

## Related Topics

- [[What-is-GitActions]] - GitHub Actions
- [[What-is-Docker]] - Containerization
- [[What-is-Deployment]] - Deployment overview
- [[What-is-Netlify]] - Netlify
- [[What-is-Vercel]] - Vercel
- [[Setup-CICD]] - Setting up CI/CD