# How to Enable Strict Mode

## Method 1: File-Level Strict Mode

```javascript
// Add at the very first line of your file
"use strict";

// All code in this file is now in strict mode
let x = 10;  // ✅ OK
y = 20;      // ❌ Error: undeclared variable
```

## Method 2: Function-Level Strict Mode

```javascript
function myFunction() {
    "use strict";
    // Only this function is in strict mode
    let x = 10;  // ✅ OK
    y = 20;      // ❌ Error
}

function otherFunction() {
    // Not in strict mode
    y = 20;  // ✅ OK (no error)
}
```

## Method 3: ES6 Modules (Automatic)

```javascript
// myModule.js - No need for "use strict"
// Modules are automatically in strict mode

export function test() {
    x = 10;  // ❌ Error (strict mode)
}

export const value = 42;
```

## Method 4: Node.js

```javascript
// In Node.js, you can use:
// 1. File-level "use strict"
// 2. ES6 modules (.mjs or "type": "module" in package.json)

// CommonJS (require)
"use strict";
const lodash = require('lodash');

// ES Modules (import)
import lodash from 'lodash';
```

## Best Practices

```javascript
// ✅ Recommended: Always use strict mode

// Option 1: Add to every file
"use strict";

// Option 2: Use ES6 modules (automatic strict mode)
// myFile.js
export function myFunction() {
    // Already in strict mode
}

// Option 3: Use in Node.js with "type": "module"
// package.json
{
    "type": "module"
}
```

## Checking If Strict Mode Is Active

```javascript
function isStrictMode() {
    return !this;
}

console.log(isStrictMode()); // false (normal mode)
// or
console.log(isStrictMode()); // true (strict mode)
```

## Common Mistakes

```javascript
// ❌ Wrong: "use strict" not at top
let x = 10;
"use strict";  // This doesn't work!

// ✅ Right: Must be first statement
"use strict";
let x = 10;

// ❌ Wrong: Missing quotes
"use strict"  // SyntaxError!

// ✅ Right: Include quotes
"use strict";
```

## Quick Revision

1. Add `"use strict";` as first line of file
2. Or use ES6 modules (automatic strict mode)
3. Must be first statement (before any code)
4. Always use strict mode in modern [[JavaScript]]
5. Node.js: Use ES modules or add "use strict"

---

## Related Topics

- [[What-is-Strict-Mode]] - What strict mode does
- [[What-is-Module]] - ES6 modules
- [[What-is-ES6]] - ES6 features
- [[What-is-Node]] - Node.js setup