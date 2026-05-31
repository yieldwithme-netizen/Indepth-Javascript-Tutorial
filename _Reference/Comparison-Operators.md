# Comparison Operators

## Definition

**Comparison operators** compare two values and return a boolean (`true` or `false`). They are essential for conditional logic, loops, and data validation in JavaScript.

## Equality Operators

### Loose Equality (`==`)

Compares values after type coercion:

```javascript
console.log(5 == "5");      // true (string coerced to number)
console.log(0 == false);    // true (boolean coerced to number)
console.log(null == undefined); // true
console.log("" == 0);       // true
console.log(" " == 0);      // true (whitespace trimmed)
```

### Strict Equality (`===`)

Compares both value **and** type (no coercion):

```javascript
console.log(5 === "5");     // false (number vs string)
console.log(0 === false);   // false (number vs boolean)
console.log(null === undefined); // false
console.log(NaN === NaN);   // false (special case!)
```

### Inequality Operators

```javascript
// Loose Inequality (!=)
console.log(5 != "5");      // false
console.log(5 != "3");      // true

// Strict Inequality (!==)
console.log(5 !== "5");     // true
console.log(5 !== 5);       // false
```

## Relational Operators

```javascript
const age = 25;

console.log(age > 18);    // true
console.log(age >= 18);   // true
console.log(age < 30);    // true
console.log(age <= 30);   // true

// With strings (lexicographic comparison)
console.log("apple" < "banana");  // true
console.log("Z" < "a");          // true (uppercase < lowercase)
```

## Logical Operators

### AND (`&&`)

```javascript
const age = 25;
const hasLicense = true;

if (age >= 18 && hasLicense) {
  console.log("You can drive");
}

// Short-circuit evaluation
const user = null;
const name = user && user.name; // Returns null (stops evaluating)
```

### OR (`||`)

```javascript
const isLoggedIn = false;
const isGuest = true;

if (isLoggedIn || isGuest) {
  console.log("Access granted");
}

// Default values
const username = "Alice";
const displayName = username || "Anonymous"; // "Alice"

const emptyName = "";
const fallbackName = emptyName || "Anonymous"; // "Anonymous"
```

### NOT (`!`)

```javascript
const isAdmin = false;
console.log(!isAdmin); // true

const items = [];
console.log(!items); // false (arrays are truthy)

// Double NOT for boolean conversion
console.log(!!5);    // true
console.log(!!0);    // false
console.log(!!"");   // false
```

## Nullish Coalescing (`??`)

Returns right operand only when left is `null` or `undefined`:

```javascript
const count = 0;
console.log(count || 10);  // 10 (0 is falsy)
console.log(count ?? 10);  // 0 (0 is not null/undefined)

const name = "";
console.log(name || "Default"); // "Default" (empty string is falsy)
console.log(name ?? "Default"); // "" (empty string is not null/undefined)

const value = null;
console.log(value ?? "Default"); // "Default"
```

## Truthy and Falsy Values

### Falsy Values

```javascript
false
0
-0
0n      // BigInt zero
""      // Empty string
''
``        // Empty template literal
null
undefined
NaN
```

### Truthy Values (Non-Falsy)

```javascript
true
1, -1, 42
"hello"
[]
{}
function() {}
Infinity
-Infinity
```

### Checking Truthiness

```javascript
const items = [];

if (items.length) {
  console.log("Has items");
} else {
  console.log("No items"); // This runs
}

// Use .length > 0 for explicit checks
if (items.length > 0) {
  console.log("Has items");
}
```

## Operator Precedence

```javascript
// && has higher precedence than ||
console.log(true || false && false); // true (evaluates as: true || (false && false))

// Comparison operators have higher precedence than && and ||
console.log(5 > 3 || 2 < 1); // true (evaluates as: (5 > 3) || (2 < 1))
```

## Common Use Cases

### Conditional Rendering

```javascript
const role = "admin";
const message = role === "admin"
  ? "Welcome, Admin"
  : role === "user"
    ? "Welcome, User"
    : "Access Denied";
```

### Validation

```javascript
function validateEmail(email) {
  return typeof email === "string" && email.includes("@") && email.length > 3;
}
```

### Default Values

```javascript
function greet(name) {
  const displayName = name ?? "Guest";
  return `Hello, ${displayName}!`;
}
```

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Using `=` instead of `===` | Always use strict equality |
| Confusing `&&` and `||` precedence | Use parentheses for clarity |
| Using `\|\|` for default numbers | Use `??` (e.g., `0 \|\| 10` gives wrong result) |
| Comparing with `NaN` | Use `Number.isNaN(value)` |

## Quick Revision

- **Always prefer `===` and `!==`** over loose equality
- `??` is better than `||` for default values (preserves falsy like `0` and `""`)
- `&&` returns first falsy or last value
- `||` returns first truthy or last value
- `!` converts to boolean and inverts
- Use `Number.isNaN()` to check for NaN

## Related Topics

- [[Truthy-Falsy]]
- [[Ternary-Operator]]
- [[Logical-Operators]]
- [[Condition-Statements]]
- [[Nullish-Coalescing]]
