# CommonJS Modules

## Definition

CommonJS is a module system used primarily in Node.js for organizing JavaScript code. It provides `require()` for importing modules and `module.exports` or `exports` for exporting functionality.

## Syntax

### Exporting

```javascript
// math.js - Single export
module.exports = function add(a, b) {
  return a + b;
};

// utils.js - Multiple exports
function capitalize(str) {
  return str.charAt(0).toUpperCase() + str.slice(1);
}

function lowercase(str) {
  return str.toLowerCase();
}

module.exports = { capitalize, lowercase };

// Or using exports directly
exports.capitalize = capitalize;
exports.lowercase = lowercase;
```

### Importing

```javascript
// Single export
const add = require("./math");

console.log(add(2, 3)); // 5

// Multiple exports
const { capitalize, lowercase } = require("./utils");

console.log(capitalize("hello")); // "Hello"
console.log(lowercase("HELLO")); // "hello"

// Importing built-in modules
const fs = require("fs");
const path = require("path");
const http = require("http");

// Importing node_modules
const express = require("express");
const lodash = require("lodash");
```

## Code Examples

### Creating a Utility Module

```javascript
// utils/validation.js
function isEmail(email) {
  const regex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return regex.test(email);
}

function isPasswordStrong(password) {
  return password.length >= 8 &&
         /[A-Z]/.test(password) &&
         /[a-z]/.test(password) &&
         /[0-9]/.test(password);
}

module.exports = { isEmail, isPasswordStrong };
```

### Using the Module

```javascript
// app.js
const { isEmail, isPasswordStrong } = require("./utils/validation");

console.log(isEmail("user@example.com")); // true
console.log(isPasswordStrong("Secure123")); // true
```

### Module Caching

```javascript
// Node.js caches modules after first require
const module1 = require("./myModule");
const module2 = require("./myModule");

console.log(module1 === module2); // true - same cached object
```

### Conditional Exports

```javascript
// platform.js
if (process.platform === "win32") {
  module.exports = require("./windows-commands");
} else if (process.platform === "darwin") {
  module.exports = require("./macos-commands");
} else {
  module.exports = require("./linux-commands");
}
```

## Common Use Cases

- Server-side Node.js applications
- CLI tools and scripts
- npm packages (though many now support ESM)
- Legacy projects before ES modules

## Common Mistakes

1. **Circular dependencies**: Can cause undefined values

```javascript
// a.js
const b = require("./b");
module.exports = { a: 1 };

// b.js
const a = require("./a"); // May be partially loaded
module.exports = { b: 2 };
```

2. **Synchronous loading**: `require()` is synchronous, can affect performance

3. **Using `module.exports` vs `exports`**: `exports` is a reference to `module.exports`

```javascript
// Wrong - overwrites exports
exports = { key: "value" };

// Correct - add properties
exports.key = "value";

// Or reassign module.exports
module.exports = { key: "value" };
```

## Related Topics

- [[ES-Modules]] - Modern ES module syntax
- [[Node.js-Modules]] - Module system in Node.js
- [[Module-Bundlers]] - Webpack, Rollup, etc.
- [[Require-vs-Import]] - CommonJS vs ESM differences
- [[Package.json]] - Package configuration

## Quick Revision Summary

| Feature | CommonJS |
|---------|----------|
| Import | `require()` |
| Export | `module.exports` / `exports` |
| Loading | Synchronous |
| Environment | Primarily Node.js |
| Syntax | Dynamic |
| Support | Node.js, older bundlers |
