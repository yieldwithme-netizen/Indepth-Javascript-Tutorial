# JavaScript Types

## Definition

JavaScript has **8 data types** divided into primitives and reference types.

## Primitives (7)

```javascript
let str = "Hello";      // string
let num = 42;           // number
let bool = true;        // boolean
let empty = null;       // null
let undef;              // undefined
let sym = Symbol();     // symbol
let big = 123n;         // bigint
```

## Reference Types

```javascript
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

- 7 primitives + reference types
- `typeof` to check type
- Primitives: immutable, by value
- Reference: mutable, by reference

---

## Related Topics

- [[What-is-DataType]] - [[What-is-DataType|Data types]]
- [[JavaScript-Types]] - [[JavaScript-Types|JavaScript types]]
- [[What-is-Primitive]] - [[What-is-Primitive|Primitives]]
- [[Variables-and-Types]] - [[Variables-and-Types|Variables and types]]
