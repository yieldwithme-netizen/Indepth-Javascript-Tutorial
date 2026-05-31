# What is CommonJS

## Definition

CommonJS is a module system for JavaScript that was originally designed for server-side development with Node.js. It provides a synchronous way to include modules, where each file is treated as a separate module with its own scope. CommonJS uses `require()` to import modules and `module.exports` or `exports` to export functionality.

CommonJS was standardized in 2009 as part of the CommonJS specification and became the default module system for Node.js. It was designed to solve the problem of JavaScript's lack of a built-in module system, allowing developers to break code into reusable, maintainable pieces.

## Syntax

### Exporting

```javascript
// math.js - CommonJS exports

// Method 1: Using module.exports directly
module.exports = {
  add: function(a, b) {
    return a + b;
  },
  subtract: function(a, b) {
    return a - b;
  }
};

// Method 2: Using exports property
exports.multiply = function(a, b) {
  return a * b;
};

exports.PI = 3.14159;
```

### Importing

```javascript
// app.js - CommonJS imports

// Import entire module
const math = require('./math.js');
console.log(math.add(2, 3)); // 5
console.log(math.multiply(4, 5)); // 20

// Destructure specific exports
const { add, subtract } = require('./math.js');
console.log(add(10, 5)); // 15

// Import built-in Node.js modules
const fs = require('fs');
const path = require('path');

// Import npm packages
const express = require('express');
const lodash = require('lodash');
```

### Default Exports

```javascript
// logger.js
class Logger {
  constructor(name) {
    this.name = name;
  }
  
  log(message) {
    console.log(`[${this.name}] ${message}`);
  }
}

module.exports = Logger;
```

```javascript
// app.js
const Logger = require('./logger.js');
const logger = new Logger('MyApp');
logger.log('Application started');
```

## Key Characteristics

### 1. Synchronous Loading

CommonJS modules are loaded synchronously, which is suitable for server-side operations but not ideal for browsers.

```javascript
// This works in Node.js but not in browsers
const config = require('./config.js');
const database = require('./database.js');
```

### 2. Module Caching

Once a module is loaded, it is cached. Subsequent `require()` calls return the cached version.

```javascript
// counter.js
let count = 0;

module.exports = {
  increment: () => ++count,
  getCount: () => count
};
```

```javascript
// app.js
const counter1 = require('./counter.js');
const counter2 = require('./counter.js');

counter1.increment();
console.log(counter2.getCount()); // 1 (same module instance)
```

### 3. Path Resolution

CommonJS resolves module paths using `require.resolve()`:

```javascript
// Relative paths
const utils = require('./utils/helper.js');

// Absolute paths
const config = require('/etc/app/config.js');

// Node.js built-in modules
const http = require('http');

// npm packages (looked up in node_modules)
const express = require('express');
```

### 4. Dynamic Imports

You can conditionally or dynamically require modules:

```javascript
// Conditional require
if (process.env.NODE_ENV === 'production') {
  const prodConfig = require('./config.production.js');
} else {
  const devConfig = require('./config.development.js');
}

// Dynamic require based on variable
function loadModule(moduleName) {
  return require(`./modules/${moduleName}.js`);
}
```

## CommonJS vs ES Modules

| Feature | CommonJS | ES Modules |
|---------|----------|------------|
| Syntax | `require()` / `module.exports` | `import` / `export` |
| Loading | Synchronous | Asynchronous |
| Caching | By value (first load) | By reference (live bindings) |
| Tree Shaking | Not supported | Supported |
| Browser Support | Not native | Native with `<script type="module">` |
| Dynamic Import | `require()` | `import()` (returns Promise) |

```javascript
// CommonJS
const { readFile } = require('fs');
module.exports = { processData };

// ES Modules
import { readFile } from 'fs';
export { processData };
```

## Common Use Cases

- **Node.js applications**: Server-side APIs, CLI tools, background services
- **npm packages**: Most npm packages are published in CommonJS format
- **Legacy projects**: Existing codebases that predate ES modules
- **Build tools**: Webpack and other bundlers understand CommonJS
- **Configuration files**: Many tools use CommonJS for config (e.g., `.babelrc`)

## Common Mistakes

1. **Forgetting that exports is a reference**
   ```javascript
   // Wrong: This breaks the exports object
   exports = { add: (a, b) => a + b };
   
   // Correct: Use module.exports
   module.exports = { add: (a, b) => a + b };
   ```

2. **Circular dependencies**
   ```javascript
   // a.js
   const b = require('./b.js');
   exports.foo = 'foo';
   
   // b.js
   const a = require('./a.js'); // May get incomplete module
   exports.bar = 'bar';
   ```

3. **Mixing CommonJS and ES Modules**
   ```javascript
   // This doesn't work in pure Node.js
   import something from './commonjs-module.js';
   
   // Use createRequire for ES Module context
   import { createRequire } from 'module';
   const require = createRequire(import.meta.url);
   ```

4. **Not handling require errors**
   ```javascript
   // Wrong: Will crash if module doesn't exist
   const missing = require('./nonexistent.js');
   
   // Correct: Wrap in try-catch
   try {
     const missing = require('./nonexistent.js');
   } catch (err) {
     console.error('Module not found:', err.message);
   }
   ```

## Quick Revision Summary

- CommonJS uses `require()` to import and `module.exports` to export
- Modules are loaded synchronously and cached after first load
- Each file is a module with its own scope
- The `exports` object is a reference to `module.exports`
- CommonJS is the default in Node.js (not natively supported in browsers)
- Best for server-side and npm packages; use ES Modules for browser code

## Related Topics

- [[ES-Modules]]
- [[Node.js]]
- [[NPM]]
- [[Build-Tools]]
- [[Webpack]]
- [[Modules]]
- [[Import]]
- [[Export]]
