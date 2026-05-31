# Variables and Types

## Definition

Variables store data and types define **what kind of data** is stored.

## Variable Keywords

```javascript
var x = 10;   // function scoped
let y = 20;   // block scoped
const z = 30; // block scoped, immutable
```

## Data Types

```javascript
// Primitives
let str = "Hello";      // string
let num = 42;           // number
let bool = true;        // boolean
let empty = null;       // null
let undef;              // undefined
let sym = Symbol();     // symbol
let big = 123n;         // bigint

// Reference
let obj = {};           // object
let arr = [];           // array
let fn = function(){};  // function
```

## Checking Types

```javascript
typeof "Hello";     // "string"
typeof 42;          // "number"
typeof true;        // "boolean"
typeof undefined;   // "undefined"
typeof null;        // "object" (bug!)
Array.isArray([]);  // true
```

## Quick Revision

- Variables: var, let, const
- Primitives: string, number, boolean, null, undefined, symbol, bigint
- Reference: object, array, function
- Use `typeof` to check type
- Always use `const`, `let` when needed

---

## Related Topics

- [[What-is-Variable]] - [[What-is-Variable|Variables]]
- [[What-is-DataType]] - [[What-is-DataType|Data types]]
- [[Variables-and-Types]] - [[Variables-and-Types|Variables and types]]
- [[What-is-Primitive]] - [[What-is-Primitive|Primitives]]
