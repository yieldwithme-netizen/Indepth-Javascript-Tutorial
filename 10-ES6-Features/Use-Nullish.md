# How to Use Nullish Coalescing `??`

## Definition
The nullish coalescing operator (`??`) is a logical operator that returns its right-hand side operand when its left-hand side operand is `null` or `undefined`. Unlike `||`, it only treats `null` and `undefined` as falsy.

## Basic Syntax

```javascript
const value = null ?? "default";
// "default"

const value2 = 0 ?? "default";
// 0 (because 0 is not null/undefined)

const value3 = "" ?? "default";
// "" (empty string is not null/undefined)
```

## Nullish vs Logical OR

```javascript
// Logical OR: treats all falsy values
const count1 = 0 || 10;      // 10 (0 is falsy)
const count2 = "" || "text"; // "text" ("" is falsy)
const count3 = false || true; // true (false is falsy)

// Nullish: only treats null/undefined
const count4 = 0 ?? 10;      // 0 (nullish check)
const count5 = "" ?? "text"; // "" (nullish check)
const count6 = false ?? true; // false (nullish check)
```

## Common Use Cases

### Default Values

```javascript
// Bad: 0 and "" become defaults
function createUser(name = "Anonymous", age = 18) {}

// Good: Only null/undefined trigger defaults
function createUser(name, age) {
  return {
    name: name ?? "Anonymous",
    age: age ?? 18
  };
}

createUser(null, 0);  // { name: "Anonymous", age: 0 }
```

### Environment Variables

```javascript
const config = {
  port: process.env.PORT ?? 3000,
  host: process.env.HOST ?? "localhost",
  debug: process.env.DEBUG ?? false
};
```

### Function Parameters

```javascript
function fetchData(url, options = {}) {
  const timeout = options.timeout ?? 5000;
  const retries = options.retries ?? 3;
  // ...
}
```

### DOM Element Defaults

```javascript
const input = document.querySelector("#name");
const value = input?.value ?? "Default Name";
```

## Chaining Nullish

```javascript
const config = {
  db: {
    host: null,
    port: 5432
  }
};

const host = config.db?.host ?? "localhost";  // "localhost"
const port = config.db?.port ?? 3306;        // 5432
```

## Common Mistakes

```javascript
// Wrong: Using || when you need 0 or ""
const count = 0;
const display = count || "No items";  // "No items" (wrong!)

// Correct: Use ?? to preserve 0 and ""
const display2 = count ?? "No items";  // 0 (correct)

// Wrong: Assuming ?? works like ||
const name = "" ?? "Anonymous";  // "" (not "Anonymous")
const isActive = false ?? true;  // false (not true)
```

## Quick Revision

- `??` returns right side only if left is `null` or `undefined`
- `||` returns right side for any falsy value (`0`, `""`, `false`, `null`, `undefined`, `NaN`)
- Use `??` when you want to distinguish between `null`/`undefined` and other falsy values
- Can be combined with optional chaining `?.`
- Introduced in ES2020

## Related Topics

- [[Use-OptionalChaining]] - Optional chaining `?.`
- [[What-is-Null]] - Understanding null and undefined
- [[Logical-Operators]] - Logical OR and AND operators
- [[Default-Parameters]] - Function default parameters
