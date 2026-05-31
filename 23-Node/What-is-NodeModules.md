# What are Node Modules

## Definition

Node Modules is a system for organizing and sharing JavaScript code. In Node.js, every JavaScript file is treated as a separate module. The `node_modules` folder contains all installed dependencies for a project, managed by npm (Node Package Manager) or yarn.

## Module Systems

### 1. CommonJS (CJS) - Node.js Default

```javascript
// Exporting
function add(a, b) {
  return a + b;
}

function multiply(a, b) {
  return a * b;
}

module.exports = { add, multiply };

// Or individual exports
exports.add = add;
exports.multiply = multiply;
```

```javascript
// Importing
const math = require('./math');
console.log(math.add(2, 3));  // 5

// Destructuring import
const { add, multiply } = require('./math');
console.log(add(2, 3));  // 5
```

### 2. ES Modules (ESM) - Modern JavaScript

```javascript
// math.js
export function add(a, b) {
  return a + b;
}

export function multiply(a, b) {
  return a * b;
}

export default class Calculator {
  // ...
}
```

```javascript
// app.js
import Calculator, { add, multiply } from './math.js';

console.log(add(2, 3));  // 5
const calc = new Calculator();
```

## The node_modules Folder

```
my-project/
├── node_modules/        # All installed dependencies
│   ├── express/
│   │   ├── index.js
│   │   ├── package.json
│   │   └── node_modules/  # Express's own dependencies
│   ├── lodash/
│   ├── react/
│   └── ...
├── package.json         # Project metadata and dependencies
├── package-lock.json    # Exact dependency versions
└── app.js
```

## Package.json

```json
{
  "name": "my-project",
  "version": "1.0.0",
  "description": "My Node.js project",
  "main": "app.js",
  "scripts": {
    "start": "node app.js",
    "dev": "nodemon app.js",
    "test": "jest",
    "build": "webpack"
  },
  "dependencies": {
    "express": "^4.18.2",
    "lodash": "^4.17.21"
  },
  "devDependencies": {
    "nodemon": "^3.0.1",
    "jest": "^29.7.0"
  },
  "engines": {
    "node": ">=18.0.0"
  }
}
```

## Installing Packages

```bash
# Install a package
npm install express

# Install and save to dependencies
npm install express --save

# Install and save to devDependencies
npm install nodemon --save-dev

# Install globally
npm install -g nodemon

# Install all dependencies from package.json
npm install

# Install specific version
npm install express@4.18.2

# Update package
npm update express

# Uninstall package
npm uninstall express
```

## Creating Your Own Module

```javascript
// myModule.js
class Logger {
  constructor(prefix) {
    this.prefix = prefix;
  }

  log(message) {
    console.log(`[${this.prefix}] ${message}`);
  }

  error(message) {
    console.error(`[${this.prefix}] ERROR: ${message}`);
  }
}

module.exports = Logger;

// app.js
const Logger = require('./myModule');

const logger = new Logger('App');
logger.log('Server started');  // [App] Server started
logger.error('Something broke');  // [App] ERROR: Something broke
```

## Module Resolution

```javascript
// Node.js resolves modules in this order:
// 1. Built-in modules (fs, path, http)
require('fs');

// 2. node_modules folder
require('express');

// 3. Relative paths
require('./utils');
require('./utils/helper');

// 4. Absolute paths
require('/home/user/project/utils');
```

## Common Use Cases

- **Utility Libraries**: lodash, moment.js, date-fns
- **Web Frameworks**: express, koa, fastify
- **Database Clients**: mongoose, sequelize, knex
- **Testing**: jest, mocha, chai
- **Build Tools**: webpack, vite, esbuild
- **Linters**: eslint, prettier

## Common Mistakes

### Forgetting to Install Dependencies

```bash
# BAD: Running code without installing
node app.js  # Error: Cannot find module 'express'

# GOOD: Install dependencies first
npm install
node app.js
```

### Not Using package.json

```bash
# BAD: Installing without saving
npm install express

# GOOD: Always use --save or package.json
npm install express --save
# Or let npm handle it automatically (default in npm 5+)
```

### Circular Dependencies

```javascript
// a.js
const b = require('./b');
module.exports = { a: 1 };

// b.js
const a = require('./a');  // Circular! Returns incomplete module
module.exports = { b: 2 };

// Solution: Restructure code to avoid circular dependencies
// Or use dependency injection
```

### Using require() in ES Modules

```javascript
// BAD: Using CommonJS syntax in .mjs file
// file.mjs
const express = require('express');  // Error!

// GOOD: Use import syntax
import express from 'express';
```

### Not Locking Dependencies

```bash
# BAD: No lock file
# Other developers may get different versions

# GOOD: Commit package-lock.json
git add package-lock.json
```

## package-lock.json

```javascript
// package-lock.json ensures exact versions
// Never edit manually
// Always commit to version control
// Regenerated when running npm install
```

## npm Scripts

```json
{
  "scripts": {
    "start": "node app.js",
    "dev": "nodemon app.js",
    "test": "jest",
    "test:watch": "jest --watch",
    "build": "webpack --mode production",
    "lint": "eslint src/",
    "format": "prettier --write src/"
  }
}
```

```bash
# Run scripts
npm start
npm run dev
npm run test
npm run build
```

## Related Topics

- [[Read-Files]]
- [[Write-Files]]
- [[What-is-State]]
- [[Choose-Framework]]

## Quick Revision

- Every JavaScript file in Node.js is a module
- **CommonJS**: `require()` and `module.exports` (default in Node.js)
- **ES Modules**: `import` and `export` (use `.mjs` extension or `type: "module"`)
- `node_modules/` contains all installed dependencies
- `package.json` defines project metadata and dependencies
- `package-lock.json` locks exact dependency versions
- Always commit `package-lock.json` to version control
- Never manually edit `node_modules` or `package-lock.json`
