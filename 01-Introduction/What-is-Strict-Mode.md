# What is Strict Mode?

## Definition

Strict mode is a **restricted variant** of [[JavaScript]] that catches common coding mistakes and unsafe actions.

## Why Use Strict Mode?

| Without Strict | With Strict |
|----------------|-------------|
| Silent errors | Throws errors |
| Bad syntax allowed | Bad syntax rejected |
| Unsafe actions allowed | Unsafe actions blocked |

## How to Enable

```javascript
// File-level (entire file)
"use strict";

// Function-level
function myFunction() {
    "use strict";
    // code here
}

// Module-level (automatic in ES6 modules)
// No need for "use strict" in .mjs files
```

## What Strict Mode Does

### 1. Catches Silent Errors

```javascript
"use strict";

// ❌ Throws error: Assignment to undeclared variable
x = 10;

// ❌ Throws error: Deleting variable
let y = 5;
delete y;

// ❌ Throws error: Duplicate parameter
function test(a, a) {}

// ❌ Throws error: Octal syntax
let num = 010;

// ❌ Throws error: Writing to read-only property
const obj = {};
Object.defineProperty(obj, 'x', { value: 42, writable: false });
obj.x = 9; // TypeError
```

### 2. Improves Performance

```javascript
"use strict";

// JavaScript can optimize better
// No "arguments.callee" (forbidden)
// No "with" statement (forbidden)
```

### 3. Prepares for Future JavaScript

```javascript
"use strict";

// Reserved words as variables (future-proofing)
let let = 10;  // Error in strict mode
```

## Common Strict Mode Errors

```javascript
"use strict";

// 1. Undeclared variables
// ❌
x = 10;
// ✅
let x = 10;

// 2. Write-only properties
// ❌
const obj = {};
Object.defineProperty(obj, 'x', { value: 1, writable: false });
obj.x = 2;
// ✅
console.log(obj.x); // Read is OK

// 3. Deleting undeletable
// ❌
delete Object.prototype;
// ✅
// Don't try to delete built-ins

// 4. Duplicate parameters
// ❌
function test(a, b, a) {}
// ✅
function test(a, b, c) {}

// 5. Octal literals
// ❌
let num = 010;
// ✅
let num = 0o10;
```

## Modern JavaScript

```javascript
// In ES6+ modules (import/export), strict mode is automatic
// myModule.js
export function test() {
    // Already in strict mode
    x = 10; // Error without let/const
}

// In scripts, you need to add it
"use strict";
function test() {
    // Now in strict mode
}
```

## Quick Revision

- Strict mode = restricted [[JavaScript]]
- Add `"use strict";` at top of file/[[function]]
- Catches: undeleted variables, duplicate params, octal syntax
- Automatic in ES6 modules
- Always use strict mode

---

## Related Topics

- [[Enable-Strict-Mode]] - How to enable
- [[What-is-JavaScript]] - JS basics
- [[What-is-ES6]] - ES6 features
- [[What-is-Module]] - Modules (auto strict)