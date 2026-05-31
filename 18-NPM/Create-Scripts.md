# How to Create npm Scripts

## Definition

npm scripts are custom commands defined in `package.json` that automate common tasks like building, testing, linting, and deploying your project. They provide a convenient way to run frequently used commands without typing long commands.

## Basic Structure

```json
{
  "scripts": {
    "start": "node index.js",
    "dev": "nodemon index.js",
    "test": "jest",
    "build": "webpack --config webpack.config.js",
    "lint": "eslint src/"
  }
}
```

## Running Scripts

```bash
# Using npm run
npm run start
npm run dev
npm run build

# Special scripts (no run needed)
npm start
npm test
npm stop
npm restart
```

## Common Script Patterns

### Development Scripts

```json
{
  "scripts": {
    "start": "node server.js",
    "dev": "nodemon server.js",
    "debug": "node --inspect server.js",
    "dev:debug": "nodemon --inspect server.js"
  }
}
```

### Build Scripts

```json
{
  "scripts": {
    "build": "webpack --mode production",
    "build:dev": "webpack --mode development",
    "build:watch": "webpack --watch",
    "clean": "rm -rf dist",
    "prebuild": "npm run clean"
  }
}
```

### Test Scripts

```json
{
  "scripts": {
    "test": "jest",
    "test:watch": "jest --watch",
    "test:coverage": "jest --coverage",
    "test:e2e": "cypress run",
    "test:unit": "jest --testPathPattern=unit"
  }
}
```

### Lint and Format Scripts

```json
{
  "scripts": {
    "lint": "eslint src/",
    "lint:fix": "eslint src/ --fix",
    "format": "prettier --write src/",
    "format:check": "prettier --check src/"
  }
}
```

## Script Chaining

```json
{
  "scripts": {
    "prestart": "echo 'Starting server...'",
    "start": "node server.js",
    "poststart": "echo 'Server started!'"
  }
}
```

### Lifecycle Hooks

```json
{
  "scripts": {
    "pretest": "npm run lint",
    "test": "jest",
    "posttest": "echo 'Tests complete'",
    "prebuild": "npm run clean",
    "build": "webpack",
    "postbuild": "npm run deploy"
  }
}
```

## Passing Arguments

```json
{
  "scripts": {
    "test": "jest",
    "test:coverage": "jest --coverage",
    "lint": "eslint",
    "lint:file": "eslint"
  }
}
```

```bash
# Run with arguments
npm run test -- --coverage
npm run lint -- src/ --fix
```

## Using Environment Variables

```json
{
  "scripts": {
    "dev": "NODE_ENV=development nodemon server.js",
    "prod": "NODE_ENV=production node server.js",
    "debug": "DEBUG=app:* node server.js"
  }
}
```

## Cross-Platform Scripts

```json
{
  "scripts": {
    "clean": "rimraf dist",
    "dev:win": "set NODE_ENV=development && nodemon server.js",
    "dev:unix": "NODE_ENV=development nodemon server.js"
  }
}
```

Install `rimraf` for cross-platform file removal:

```bash
npm install --save-dev rimraf
```

## Complex Scripts

```json
{
  "scripts": {
    "validate": "npm run lint && npm run test && npm run build",
    "deploy": "npm run build && aws s3 sync dist/ s3://bucket",
    "release": "npm version patch && git push --tags"
  }
}
```

## Script Naming Conventions

```json
{
  "scripts": {
    "start": "node index.js",
    "dev": "nodemon index.js",
    "build": "webpack",
    "test": "jest",
    "lint": "eslint src/",
    "format": "prettier --write .",
    "clean": "rm -rf dist",
    "deploy": "bash scripts/deploy.sh"
  }
}
```

## Common Use Cases

- Development server startup
- Building and bundling code
- Running tests
- Linting and formatting
- Database migrations
- Deployment automation
- Code generation

## Common Mistakes

- Not using `run` for custom scripts
- Creating overly complex scripts
- Not using cross-platform solutions
- Ignoring lifecycle hooks
- Hardcoding paths
- Not documenting script purposes

## Related Topics

- [[What-is-PackageJSON]]
- [[Install-Packages]]
- [[What-is-DevDeps]]
- [[NPM]]
- [[Build-Tools]]
- [[Testing]]
- [[CI-CD]]

## Quick Revision

| Script | Purpose |
|--------|---------|
| `start` | Run production server |
| `dev` | Run development server |
| `test` | Run test suite |
| `build` | Build for production |
| `lint` | Check code quality |
| `format` | Format code style |
| `pre*` | Runs before script |
| `post*` | Runs after script |
