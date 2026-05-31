# Set Up CI/CD

## Definition

CI/CD (Continuous Integration/Continuous Deployment) is **automating code testing and deployment** to ensure code quality and deliver updates frequently and reliably.

## CI/CD Pipeline Stages

| Stage | Purpose |
|-------|---------|
| Build | Compile and bundle code |
| Test | Run automated tests |
| Lint | Check code quality |
| Security | Scan for vulnerabilities |
| Deploy | Push to production |

## Code Examples

### 1. GitHub Actions Workflow

```yaml
# .github/workflows/ci.yml
name: CI/CD Pipeline

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        node-version: [18.x, 20.x]

    steps:
      - uses: actions/checkout@v4

      - name: Use Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Lint
        run: npm run lint

      - name: Test
        run: npm test

      - name: Build
        run: npm run build

      - name: Upload coverage
        uses: codecov/codecov-action@v3
        with:
          file: ./coverage/coverage-final.json
```

### 2. Deploy to Vercel via GitHub Actions

```yaml
# .github/workflows/deploy-vercel.yml
name: Deploy to Vercel

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Install Vercel CLI
        run: npm install -g vercel

      - name: Pull Vercel environment
        run: vercel pull --yes --environment=production --token=${{ secrets.VERCEL_TOKEN }}

      - name: Build
        run: vercel build --prod --token=${{ secrets.VERCEL_TOKEN }}

      - name: Deploy
        run: vercel deploy --prebuilt --prod --token=${{ secrets.VERCEL_TOKEN }}
```

### 3. Deploy to Netlify via GitHub Actions

```yaml
# .github/workflows/deploy-netlify.yml
name: Deploy to Netlify

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: 20

      - name: Install dependencies
        run: npm ci

      - name: Build
        run: npm run build

      - name: Deploy to Netlify
        uses: nwtgck/actions-netlify@v2
        with:
          publish-dir: ./dist
          production-deploy: true
        env:
          NETLIFY_AUTH_TOKEN: ${{ secrets.NETLIFY_AUTH_TOKEN }}
          NETLIFY_SITE_ID: ${{ secrets.NETLIFY_SITE_ID }}
```

### 4. GitLab CI/CD Pipeline

```yaml
# .gitlab-ci.yml
stages:
  - test
  - build
  - deploy

test:
  stage: test
  image: node:20
  script:
    - npm ci
    - npm run lint
    - npm test
  coverage: '/All files\s*\|\s*([\d\.]+)/'

build:
  stage: build
  image: node:20
  script:
    - npm ci
    - npm run build
  artifacts:
    paths:
      - dist/
    expire_in: 1 hour

deploy:
  stage: deploy
  image: node:20
  script:
    - npm install -g vercel
    - vercel deploy --prebuilt --prod --token=$VERCEL_TOKEN
  only:
    - main
```

### 5. Jenkins Pipeline

```groovy
// Jenkinsfile
pipeline {
    agent any

    environment {
        NODEJS_HOME = tool 'NodeJS 20'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install') {
            steps {
                sh 'npm ci'
            }
        }

        stage('Lint') {
            steps {
                sh 'npm run lint'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test'
                junit 'test-results/*.xml'
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                sh 'npm install -g vercel'
                sh 'vercel deploy --prebuilt --prod --token=$VERCEL_TOKEN'
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
```

### 6. CircleCI Configuration

```yaml
# .circleci/config.yml
version: 2.1

orbs:
  node: circleci/node@5.0

jobs:
  test:
    executor:
      name: node/default
      tag: '20'
    steps:
      - checkout
      - node/install-packages:
          pkg-manager: npm
      - run:
          name: Lint
          command: npm run lint
      - run:
          name: Test
          command: npm test
      - store_test_results:
          path: test-results

  deploy:
    executor:
      name: node/default
      tag: '20'
    steps:
      - checkout
      - node/install-packages:
          pkg-manager: npm
      - run:
          name: Build
          command: npm run build
      - run:
          name: Deploy
          command: |
            npm install -g vercel
            vercel deploy --prebuilt --prod --token=$VERCEL_TOKEN

workflows:
  build-and-deploy:
    jobs:
      - test
      - deploy:
          requires:
            - test
          filters:
            branches:
              only: main
```

### 7. Pre-commit Hooks with Husky

```javascript
// package.json
{
  "devDependencies": {
    "husky": "^8.0.0",
    "lint-staged": "^13.0.0"
  },
  "scripts": {
    "prepare": "husky install"
  },
  "lint-staged": {
    "*.{js,ts,jsx,tsx}": ["eslint --fix", "prettier --write"],
    "*.{json,md}": ["prettier --write"]
  }
}

// .husky/pre-commit
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"
npx lint-staged
```

### 8. Docker CI/CD Pipeline

```yaml
# .github/workflows/docker.yml
name: Docker CI/CD

on:
  push:
    branches: [main]

jobs:
  docker:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3

      - name: Login to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      - name: Build and push
        uses: docker/build-push-action@v5
        with:
          context: .
          push: true
          tags: myapp:latest,myapp:${{ github.sha }}
          cache-from: type=gha
          cache-to: type=gha,mode=max
```

## Common Use Cases

```bash
# Run CI locally
npm test
npm run lint
npm run build

# Check coverage
npm test -- --coverage

# Type checking
npm run typecheck

# Security audit
npm audit
npm audit fix
```

## Common Mistakes

| Mistake | Risk |
|---------|------|
| Not caching dependencies | Slow builds |
| Missing test stage | Uncaught bugs |
| Secrets in code | Security breach |
| No rollback strategy | Failed deployments |
| Ignoring build failures | Broken production |
| Not running tests on PR | Merging broken code |
| Missing environment checks | Config issues |

## Quick Revision

- CI = Continuous Integration (test on every commit)
- CD = Continuous Deployment (auto-deploy to production)
- Always run tests before deployment
- Cache dependencies for faster builds
- Use secrets management for sensitive data
- Set up branch protection rules
- Monitor pipeline health
- Implement rollback procedures
- Use pre-commit hooks for code quality
- Document the pipeline for team

---

## Related Topics

- [[What-is-CICD]] - CI/CD concepts
- [[What-is-GitActions]] - GitHub Actions
- [[Deploy-Vercel]] - Vercel deployment
- [[Deploy-Netlify]] - Netlify deployment
- [[What-is-Docker]] - Docker containers
- [[What-is-Deployment]] - Deployment overview
