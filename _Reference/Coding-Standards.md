# Coding Standards

## Definition
Coding standards are established guidelines for writing consistent, readable, and maintainable code. They define naming conventions, formatting rules, and best practices for a codebase.

## Code Style Guide (Based on Airbnb/Google Standards)

### Naming Conventions
```javascript
// Variables and functions: camelCase
const userName = 'John';
function getUserData() {}

// Classes and constructors: PascalCase
class UserAccount {}
function UserAccount() {}

// Constants: UPPER_SNAKE_CASE
const MAX_RETRY_COUNT = 3;
const API_BASE_URL = 'https://api.example.com';

// Boolean variables: prefix with is/has/can
const isActive = true;
const hasPermission = false;
const canEdit = true;

// Private properties: prefix with underscore
class User {
  _privateMethod() {}
  _internalState = {};
}

// Files: kebab-case or camelCase
// userAccount.js or user-account.js
```

### Formatting
```javascript
// 2-space indentation
function example() {
  if (true) {
    return 1;
  }
}

// Single quotes for strings
const name = 'Alice';

// No trailing semicolons (or always use them — pick one)
const value = 'test';

// Trailing commas in multiline
const config = {
  host: 'localhost',
  port: 3000,
  debug: true,
};

// Space before curly braces
if (condition) {
  doSomething();
}

// No space before function parentheses
function greet() {}
const greet = () => {};
```

### Variable Declarations
```javascript
// Use const by default, let when reassignment is needed
const API_URL = 'https://api.example.com';
let counter = 0;
counter += 1;

// Never use var
// var oldWay = 'avoid this';

// Destructuring
const { name, age } = user;
const [first, second] = array;
```

### Functions
```javascript
// Arrow functions for callbacks
const numbers = [1, 2, 3].map(n => n * 2);

// Named functions for top-level declarations
function processData(data) {
  // ...
}

// Default parameters
function greet(name = 'Guest') {
  return `Hello, ${name}`;
}

// Rest parameters
function sum(...numbers) {
  return numbers.reduce((a, b) => a + b, 0);
}
```

### Error Handling
```javascript
// Always handle errors
async function fetchData(url) {
  try {
    const response = await fetch(url);
    if (!response.ok) throw new Error(`HTTP ${response.status}`);
    return await response.json();
  } catch (error) {
    console.error('Fetch failed:', error.message);
    throw error;
  }
}

// Use custom error classes
class ValidationError extends Error {
  constructor(message, field) {
    super(message);
    this.name = 'ValidationError';
    this.field = field;
  }
}
```

### Imports and Exports
```javascript
// Named imports grouped logically
import React from 'react';
import { useState, useEffect } from 'react';

import { fetchUser } from './api/user';
import { formatDate } from './utils/date';

import './styles.css';
```

## Linting Setup

### ESLint Configuration
```json
// .eslintrc.json
{
  "extends": ["eslint:recommended"],
  "env": {
    "browser": true,
    "node": true,
    "es2022": true
  },
  "rules": {
    "no-unused-vars": "error",
    "no-console": "warn",
    "eqeqeq": "error",
    "curly": "error",
    "prefer-const": "error",
    "no-var": "error"
  }
}
```

### Prettier Configuration
```json
// .prettierrc
{
  "semi": true,
  "singleQuote": true,
  "tabWidth": 2,
  "trailingComma": "es5",
  "printWidth": 80,
  "bracketSpacing": true
}
```

## Common Use Cases
- Onboarding new team members consistently
- Reducing code review friction
- Enforcing security best practices
- Maintaining large codebases
- Automating formatting with tools

## Common Mistakes
- Not having a linter configured
- Mixing coding styles across the project
- Inconsistent semicolon usage
- Overly complex one-liners that sacrifice readability
- Not documenting the agreed-upon standards

## Related Topics
- [[ESLint]]
- [[Prettier]]
- [[Code-Comments]]
- [[Naming-Conventions]]
- [[Code-Style]]
- [[Refactoring]]

## Quick Revision
- Use `const` by default, `let` when needed, never `var`
- camelCase for variables/functions, PascalCase for classes
- Enforce standards with ESLint and Prettier
- Prefer readable code over clever code
- Keep functions small and focused
- Handle errors explicitly
