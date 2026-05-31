# Module Pattern

## Definition

The module pattern **encapsulates code** into private and public scopes using closures.

## Basic Syntax

```javascript
const Module = (function() {
    // Private
    let privateVar = 0;
    
    function privateMethod() {
        console.log("Private");
    }
    
    // Public
    return {
        publicVar: "public",
        publicMethod() {
            privateVar++;
            privateMethod();
        }
    };
})();

Module.publicMethod(); // "Private"
Module.publicVar; // "public"
Module.privateVar; // undefined
```

## Revealing Module Pattern

```javascript
const Calculator = (function() {
    let result = 0;
    
    function add(a, b) {
        return a + b;
    }
    
    function subtract(a, b) {
        return a - b;
    }
    
    return {
        add,
        subtract,
        getResult: () => result
    };
})();

Calculator.add(5, 3); // 8
Calculator.result; // undefined (private)
```

## Modern ES6 Modules

```javascript
// math.js
const PI = 3.14159; // private
export function add(a, b) { return a + b; } // public

// app.js
import { add } from './math.js';
add(1, 2); // 3
PI; // undefined
```

## Quick Revision

- Module pattern = encapsulation with closures
- Private variables/methods hidden
- Public API returned
- Modern alternative: ES6 modules
- Use for: privacy, encapsulation

---

## Related Topics

- [[What-is-Module]] - [[What-is-Module|Modules]]
- [[What-is-Closure]] - [[What-is-Closure|Closures]]
- [[What-is-IIFE]] - [[What-is-IIFE|IIFE]]
- [[What-is-Scope]] - [[What-is-Scope|Scope]]
- [[What-is-Encapsulation]] - [[What-is-Encapsulation|Encapsulation]]
