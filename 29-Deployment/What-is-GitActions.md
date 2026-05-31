# What is GitHub Actions?

## Definition

GitHub Actions is a **CI/CD platform built into GitHub** that automates build, test, and deployment workflows directly from your repository.

## Core Concepts

| Concept | Description |
|---------|-------------|
| **Workflow** | Automated process defined in YAML |
| **Event** | Trigger (push, PR, schedule, etc.) |
| **Job** | Set of steps running on same runner |
| **Step** | Individual task within a job |
| **Action** | Reusable code unit |
| **Runner** | Server that executes workflows |

## Basic Workflow Structure

```yaml
# .github/workflows/ci.yml
name: CI Workflow

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Run linting
        run: npm run lint

      - name: Run tests
        run: npm test

      - name: Build application
        run: npm run build
```

## Common Triggers

```yaml
# On push to main
on:
  push:
    branches: [main]

# On pull request
on:
  pull_request:
    branches: [main]

# On schedule (cron)
on:
  schedule:
    - cron: '0 2 * * 1'  # Every Monday at 2 AM

# On workflow dispatch (manual)
on:
  workflow_dispatch:
    inputs:
      environment:
        description: 'Deployment environment'
        required: true
        default: 'staging'
```

## Deploy to Vercel Example

```yaml
name: Deploy to Vercel

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Install Vercel CLI
        run: npm install --global vercel

      - name: Pull Vercel Environment
        run: vercel pull --yes --environment=production --token=${{ secrets.VERCEL_TOKEN }}

      - name: Build
        run: vercel build --prod

      - name: Deploy to Production
        id: deploy
        run: vercel deploy --prebuilt --prod --token=${{ secrets.VERCEL_TOKEN }}
```

## Environment Variables and Secrets

```yaml
# Use repository secrets
env:
  API_KEY: ${{ secrets.API_KEY }}
  DATABASE_URL: ${{ secrets.DATABASE_URL }}

# Use environment-specific secrets
jobs:
  deploy:
    environment: production
    steps:
      - run: echo "Deploying to ${{ vars.ENVIRONMENT }}"
```

## Caching for Speed

```yaml
steps:
  - uses: actions/cache@v3
    with:
      path: ~/.npm
      key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}

  - run: npm ci
```

## Common Mistakes

```yaml
# BAD: Hardcoded secrets
- run: curl -H "Authorization: Bearer abc123" https://api.example.com

# GOOD: Use secrets
- run: curl -H "Authorization: Bearer ${{ secrets.API_KEY }}" https://api.example.com

# BAD: Not pinning action versions
- uses: actions/checkout@main

# GOOD: Pin specific version
- uses: actions/checkout@v3

# BAD: Running unnecessary steps
jobs:
  build:
    steps:
      - run: npm install
      - run: npm test
      - run: npm run build

# GOOD: Use conditional steps
jobs:
  build:
    steps:
      - run: npm ci
      - run: npm test
      - run: npm run build
        if: success()
```

## Quick Revision

- GitHub Actions automates CI/CD workflows
- Workflows are defined in `.github/workflows/` YAML files
- Common triggers: push, pull_request, schedule
- Use `secrets` for sensitive data
- Cache dependencies to speed up workflows
- Use matrix strategies for multi-platform testing

---

## Related Topics

- [[What-is-CICD]] - CI/CD concepts
- [[What-is-Vercel]] - Vercel deployment
- [[What-is-Netlify]] - Netlify deployment
- [[What-is-Docker]] - Containerization
- [[What-is-Deployment]] - Deployment overview
- [[Setup-CICD]] - Setting up CI/CD