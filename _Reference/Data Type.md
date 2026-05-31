# Data Types

## Definition

JavaScript has **8 data types** divided into two categories: **primitive** types (immutable values) and **reference** types (mutable objects). Understanding data types is fundamental to writing correct and predictable code.

## Primitive Types (7)

### 1. String

Textual data enclosed in quotes:

```javascript
const name = "Alice";          // Double quotes
const greeting = 'Hello!';     // Single quotes
const message = `Hi, ${name}`; // Template literals (backticks)

console.log(typeof name); // "string"
console.log(name.length); // 5
console.log(name[0]);     // "A"
```

### 2. Number

Both integers and floating-point numbers:

```javascript
const age = 25;            // Integer
const price = 9.99;        // Float
const negative = -100;     // Negative
const infinity = Infinity; // Special value
const notANumber = NaN;    // Not a Number

console.log(typeof age);       // "number"
console.log(typeof Infinity);  // "number"
console.log(typeof NaN);       // "number"
```

### 3. BigInt

Arbitrary-precision integers:

```javascript
const big = 123456789012345678901234567890n;
const alsoBig = BigInt("123456789012345678901234567890");

console.log(typeof big); // "bigint"

// Cannot mix with regular numbers
// big + 1; // TypeError
big + 1n; // Works
```

### 4. Boolean

Logical `true` or `false`:

```javascript
const isActive = true;
const isDeleted = false;

console.log(typeof isActive); // "boolean"

// Common in comparisons
console.log(5 > 3);       // true
console.log(5 === "5");   // false
```

### 5. undefined

Variable declared but not assigned:

```javascript
let x;
console.log(x);          // undefined
console.log(typeof x);   // "undefined"

const obj = {};
console.log(obj.missing); // undefined
```

### 6. null

Intentional absence of value:

```javascript
const user = null;
console.log(typeof user); // "object" (known JS bug!)

// Difference from undefined
let a;          // undefined (uninitialized)
let b = null;   // null (intentionally empty)
```

### 7. Symbol

Unique identifiers (ES6):

```javascript
const sym1 = Symbol("id");
const sym2 = Symbol("id");

console.log(sym1 === sym2); // false (each Symbol is unique)
console.log(typeof sym1);   // "symbol"

// Used as object keys
const ID = Symbol("id");
const user = {
  [ID]: 123,
  name: "Alice",
};
```

## Reference Types (1)

### Object

Collections of key-value pairs:

```javascript
const person = {
  name: "Alice",
  age: 25,
  greet() {
    return `Hi, I'm ${this.name}`;
  },
};

console.log(typeof person); // "object"
console.log(person.name);   // "Alice"
```

### Special Object Types

```javascript
// Array
const colors = ["red", "green", "blue"];
console.log(typeof colors); // "object"

// Function
const add = (a, b) => a + b;
console.log(typeof add); // "function"

// Date
const now = new Date();

// RegExp
const pattern = /hello/i;

// Map and Set
const map = new Map();
const set = new Set();
```

## Type Checking

### `typeof` Operator

```javascript
console.log(typeof "hello");     // "string"
console.log(typeof 42);          // "number"
console.log(typeof true);        // "boolean"
console.log(typeof undefined);   // "undefined"
console.log(typeof null);        // "object" (bug)
console.log(typeof {});          // "object"
console.log(typeof []);          // "object"
console.log(typeof function(){}); // "function"
console.log(typeof Symbol());    // "symbol"
```

### More Reliable Checks

```javascript
// Check for null
const value = null;
value === null; // true

// Check for array
Array.isArray([]); // true
Array.isArray({}); // false

// Check for plain object
Object.prototype.toString.call({}) === "[object Object]"; // true
```

## Type Coercion

JavaScript automatically converts types in certain contexts:

### Implicit Coercion

```javascript
// String concatenation
"5" + 3;       // "53" (number coerced to string)
"5" + true;    // "5true"

// Numeric operations
"5" - 3;       // 2 (string coerced to number)
"5" * "2";     // 10

// Boolean coercion
if ("hello") { ... }  // truthy
if (0) { ... }        // falsy
```

### Explicit Coercion

```javascript
// To string
String(123);         // "123"
(123).toString();    // "123"
`${123}`;            // "123"

// To number
Number("42");        // 42
parseInt("42px");    // 42
parseFloat("3.14");  // 3.14
+"5";               // 5

// To boolean
Boolean(0);          // false
Boolean("hello");    // true
!!0;                 // false
!!"hello";           // true
```

## Pass by Value vs Reference

```javascript
// Primitives - copied by value
let a = 10;
let b = a;
b = 20;
console.log(a); // 10 (unchanged)

// Objects - copied by reference
let obj1 = { x: 10 };
let obj2 = obj1;
obj2.x = 20;
console.log(obj1.x); // 20 (changed!)

// Create independent copy
const obj3 = { ...obj1 }; // Spread operator
const obj4 = JSON.parse(JSON.stringify(obj1)); // Deep copy
```

## Common Use Cases

```javascript
// typeof for checking existence
if (typeof variable !== "undefined") {
  // variable exists
}

// Type validation
function validateInput(value) {
  if (typeof value === "string") return value.trim();
  if (typeof value === "number") return value.toString();
  return String(value);
}

// Default values
function greet(name = "Guest") {
  return `Hello, ${name}!`;
}
```

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| `typeof null` returns "object" | Use `=== null` for null checks |
| Confusing `==` and `===` | Always use strict equality |
| Comparing objects with `===` | Compare properties or use deep equality |
| `typeof []` returns "object" | Use `Array.isArray()` |

## Quick Revision

| Type | typeof | Example |
|------|--------|---------|
| String | "string" | `"hello"` |
| Number | "number" | `42`, `3.14` |
| BigInt | "bigint" | `123n` |
| Boolean | "boolean" | `true`, `false` |
| undefined | "undefined" | `undefined` |
| null | "object" | `null` |
| Symbol | "symbol" | `Symbol("id")` |
| Object | "object" | `{}`, `[]`, `new Date()` |

- **Primitives** are immutable and compared by value
- **Reference types** are mutable and compared by reference
- Always use `===` to avoid coercion issues
- Use `typeof` for type checking (except for `null` and arrays)

## Related Topics

- [[Type-Coercion]]
- [[Truthy-Falsy]]
- [[Comparison-Operators]]
- [[Variables]]
- [[Objects]]
- [[Arrays]]
- [[TypeScript]]
