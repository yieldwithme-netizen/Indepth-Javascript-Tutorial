# What is package.json

## Definition

`package.json` is a metadata file that defines a Node.js project. It contains information about the project including its name, version, dependencies, scripts, and configuration. Every Node.js project should have a `package.json` file at its root directory.

## Basic Structure

```json
{
  "name": "my-project",
  "version": "1.0.0",
  "description": "A sample Node.js project",
  "main": "index.js",
  "scripts": {
    "start": "node index.js",
    "dev": "nodemon index.js",
    "test": "jest"
  },
  "keywords": ["javascript", "node"],
  "author": "John Doe",
  "license": "MIT",
  "dependencies": {
    "express": "^4.18.2"
  },
  "devDependencies": {
    "nodemon": "^3.0.1",
    "jest": "^29.7.0"
  }
}
```

## Key Fields Explained

### name and version

```json
{
  "name": "my-awesome-package",
  "version": "1.2.3"
}
```

- **name**: Package name (lowercase, no spaces, max 214 characters)
- **version**: Semantic versioning (major.minor.patch)

### main Entry Point

```json
{
  "main": "src/index.js"
}
```

Defines the entry point when your package is imported.

### scripts

```json
{
  "scripts": {
    "start": "node server.js",
    "dev": "nodemon server.js",
    "build": "webpack --config webpack.config.js",
    "test": "jest --coverage",
    "lint": "eslint src/",
    "format": "prettier --write src/"
  }
}
```

### dependencies

```json
{
  "dependencies": {
    "express": "^4.18.2",
    "lodash": "^4.17.21",
    "axios": "~1.6.0"
  }
}
```

Runtime dependencies required for the application to work.

### devDependencies

```json
{
  "devDependencies": {
    "nodemon": "^3.0.1",
    "jest": "^29.7.0",
    "eslint": "^8.50.0"
  }
}
```

Development-only dependencies (testing, linting, building tools).

### engines

```json
{
  "engines": {
    "node": ">=14.0.0",
    "npm": ">=6.0.0"
  }
}
```

Specifies required Node.js and npm versions.

### repository

```json
{
  "repository": {
    "type": "git",
    "url": "https://github.com/user/repo.git"
  }
}
```

### private

```json
{
  "private": true
}
```

Prevents accidental publishing to npm registry.

## Common Use Cases

- Managing project dependencies
- Defining build and development scripts
- Configuring project metadata for publishing
- Setting up project-specific tool configurations
- Managing different environments (dev, test, prod)

## Common Mistakes

- Committing `node_modules` directory to version control
- Not using semantic versioning for your own packages
- Mixing dependencies and devDependencies
- Not specifying engine requirements
- Using absolute paths in scripts
- Not locking dependency versions properly

## Related Topics

- [[Init-Project]]
- [[Install-Packages]]
- [[What-is-DevDeps]]
- [[What-is-Lock]]
- [[Create-Scripts]]
- [[NPM]]
- [[Node-Modules]]

## Quick Revision

| Field | Purpose |
|-------|---------|
| name | Package identifier |
| version | Semantic version number |
| main | Entry point file |
| scripts | Command shortcuts |
| dependencies | Runtime packages |
| devDependencies | Development packages |
| engines | Required Node/npm versions |
| private | Prevent npm publishing |
