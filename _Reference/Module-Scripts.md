# Module Scripts

## Definition

Module scripts allow JavaScript code to be organized into separate, reusable files that can be imported and exported. ES6 modules provide a standardized way to share code between files, enabling better code organization, encapsulation, and dependency management. Module scripts are loaded with `type="module"` in HTML.

## Syntax

```html
<script type="module" src="app.js"></script>
```

```javascript
// Exporting
export const name = "value";
export function myFunction() {}
export default class MyClass {}

// Importing
import { name } from './module.js';
import MyClass from './module.js';
import * as Module from './module.js';
```

## Code Examples

### Basic Module Export and Import

```javascript
// math.js - Exporting functions
export const PI = 3.14159;

export function add(a, b) {
  return a + b;
}

export function subtract(a, b) {
  return a - b;
}

// app.js - Importing
import { PI, add, subtract } from './math.js';

console.log(PI);           // 3.14159
console.log(add(2, 3));    // 5
console.log(subtract(5, 2)); // 3
```

### Default Exports

```javascript
// Person.js
export default class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  greet() {
    return `Hello, I'm ${this.name}`;
  }
}

// app.js
import Person from './Person.js';

const john = new Person("John", 25);
console.log(john.greet()); // "Hello, I'm John"
```

### Named vs Default Exports

```javascript
// utils.js
export const formatDate = (date) => date.toLocaleDateString();
export const formatTime = (date) => date.toLocaleTimeString();

export default class DateTimeFormatter {
  format(date) {
    return `${formatDate(date)} ${formatTime(date)}`;
  }
}

// app.js
import DateTimeFormatter, { formatDate, formatTime } from './utils.js';
```

### Importing Everything

```javascript
// math.js
export const PI = 3.14159;
export function add(a, b) { return a + b; }
export function multiply(a, b) { return a * b; }

// app.js
import * as Math from './math.js';

console.log(Math.PI);           // 3.14159
console.log(Math.add(2, 3));    // 5
console.log(Math.multiply(4, 5)); // 20
```

### Dynamic Imports

```javascript
// Lazy loading modules
async function loadModule() {
  const module = await import('./heavy-module.js');
  module.init();
}

// Conditional imports
if (condition) {
  const module = await import('./feature.js');
  module.enable();
}
```

### HTML Module Script Usage

```html
<!DOCTYPE html>
<html>
<head>
  <script type="module" src="app.js"></script>
</head>
<body>
  <div id="app"></div>
  <script type="module">
    import { greet } from './utils.js';
    document.getElementById('app').textContent = greet();
  </script>
</body>
</html>
```

### Re-exporting

```javascript
// features.js - Aggregating exports
export { add, subtract } from './math.js';
export { default as Logger } from './logger.js';
export { formatDate } from './utils.js';
```

## Module Characteristics

1. **Strict mode by default** - No need for `"use strict"`
2. **Scoped to file** - Variables not in global scope
3. **Deferred execution** - Run after HTML parsing
4. **CORS required** - Must be served with proper CORS headers
5. **Single instance** - Modules cached after first load

## Common Use Cases

1. **Code organization** - Split large applications into manageable files
2. **Library creation** - Package reusable functionality
3. **Dependency management** - Clear import/export relationships
4. **Code splitting** - Load code on demand
5. **Namespace isolation** - Prevent global scope pollution

## Common Mistakes

1. **Missing `type="module"`** - Regular scripts don't support import/export
2. **CORS errors** - Can't load modules from `file://` protocol
3. **Circular dependencies** - Avoid importing modules that import each other
4. **Export before import** - Exports must be hoisted

```javascript
// WRONG: Trying to use modules in regular script
<script src="app.js"></script>  // Won't work with import/export

// RIGHT: Use module type
<script type="module" src="app.js"></script>
```

## Quick Revision Summary

- Modules use `type="module"` in script tags
- `export` shares code, `import` uses shared code
- Default exports allow single main export per file
- Named exports allow multiple exports
- Modules run in strict mode and have their own scope
- Dynamic imports enable lazy loading

## Related Topics

- [[ES6-Features]]
- [[Arrow-Functions]]
- [[Destructuring-Assignment]]
- [[Fetch-API]]
- [[Async-Await]]
- [[Closures]]
