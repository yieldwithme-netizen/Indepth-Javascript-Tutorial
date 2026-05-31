# CI/CD (Continuous Integration / Continuous Deployment)

## Definition
CI/CD is a software development practice where code changes are automatically built, tested, and deployed. **CI (Continuous Integration)** involves frequently merging code into a shared repository with automated tests. **CD (Continuous Deployment)** automates releasing validated code to production.

## CI/CD Pipeline Stages

```
Code Push → Build → Test → Deploy to Staging → Verify → Deploy to Production
```

## Setting Up CI/CD with GitHub Actions

### Basic Workflow
```yaml
# .github/workflows/ci.yml
name: CI Pipeline

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  build-and-test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Run linter
        run: npm run lint

      - name: Run tests
        run: npm test

      - name: Build
        run: npm run build
```

### Deployment Job
```yaml
  deploy:
    needs: build-and-test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Deploy to production
        run: |
          echo "Deploying to production..."
          # Add deployment commands here
        env:
          DEPLOY_TOKEN: ${{ secrets.DEPLOY_TOKEN }}
```

## Using CircleCI

```yaml
# .circleci/config.yml
version: 2.1

jobs:
  build-and-test:
    docker:
      - image: cimg/node:20.0
    steps:
      - checkout
      - restore_cache:
          keys:
            - v1-dependencies-{{ checksum "package.json" }}
      - run:
          name: Install dependencies
          command: npm ci
      - save_cache:
          paths:
            - node_modules
          key: v1-dependencies-{{ checksum "package.json" }}
      - run:
          name: Run tests
          command: npm test
      - store_test_results:
          path: test-results

workflows:
  build-test-deploy:
    jobs:
      - build-and-test
```

## Using Jenkins (Jenkinsfile)

```groovy
pipeline {
    agent any

    stages {
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
                sh 'npm run deploy'
            }
        }
    }
    post {
        failure {
            slackSend channel: '#alerts', message: 'Build failed'
        }
    }
}
```

## JavaScript Test Script Integration

```json
{
  "scripts": {
    "test": "jest --coverage",
    "test:watch": "jest --watch",
    "lint": "eslint src/",
    "build": "webpack --mode production",
    "predeploy": "npm run build",
    "deploy": "gh-pages -d dist"
  }
}
```

## Environment Variables in CI/CD

```yaml
# GitHub Actions
- name: Run tests
  run: npm test
  env:
    NODE_ENV: test
    API_KEY: ${{ secrets.API_KEY }}
```

```javascript
// Access in code
const apiKey = process.env.API_KEY;
const nodeEnv = process.env.NODE_ENV;
```

## Common Use Cases
- Automated testing on every pull request
- Auto-deploying to staging on feature branch merges
- Production releases on main branch push
- Nightly build and test runs
- Security scanning and dependency audits

## Common Mistakes
- Not caching dependencies (slows pipeline)
- Skipping tests to speed up deployment
- Hardcoding secrets in workflow files
- Not setting up proper branch protection rules
- Ignoring flaky tests that cause false failures
- Not monitoring deployment health after release

## Related Topics
- [[Testing]]
- [[Git]]
- [[NPM]]
- [[Docker]]
- [[Environment-Variables]]
- [[Git-Branching]]
- [[Deployment]]

## Quick Revision
- CI = automatically build and test code on every change
- CD = automatically deploy validated code to production
- GitHub Actions, CircleCI, and Jenkins are popular CI/CD tools
- Always cache dependencies and store test results
- Use secrets management for sensitive configuration
- Set up branch protection and required status checks
