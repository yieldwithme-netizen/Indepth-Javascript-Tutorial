# Logical Operators

## Definition

**Logical operators** are used to combine or invert boolean expressions in JavaScript. They are essential for controlling program flow, making decisions, and evaluating complex conditions.

JavaScript has four main logical operators: AND (`&&`), OR (`||`), NOT (`!`), and Nullish Coalescing (`??`). These operators work with truthy and falsy values, not just strict booleans.

---

## Syntax

```javascript
// AND - returns first falsy or last value
expr1 && expr2

// OR - returns first truthy or last value
expr1 || expr2

// NOT - inverts boolean value
!expr

// Nullish Coalescing - returns right side only if left is null/undefined
expr1 ?? expr2
```

---

## Truthy and Falsy Values

```javascript
// Falsy values (evaluate to false in boolean context)
false
0
-0
0n (BigInt zero)
"" (empty string)
null
undefined
NaN

// Truthy values (everything else)
true
42
-42
"hello"
[] (empty array)
{} (empty object)
function() {}
```

---

## Code Examples

### AND Operator (&&)
```javascript
// Basic usage
const a = true && true;      // true
const b = true && false;     // false
const c = false && true;     // false
const d = false && false;    // false

// Short-circuit evaluation
const x = 0 && console.log('This won\'t print');
// Returns 0 (falsy), stops evaluation

// Chaining
const result = 1 && 2 && 3;  // Returns 3 (last truthy value)
const fail = 1 && 0 && 3;    // Returns 0 (first falsy value)

// Practical use: guard clause
function greet(name) {
  return name && `Hello, ${name}!`;
}

console.log(greet('Alice')); // Output: Hello, Alice!
console.log(greet(''));      // Output: ''
console.log(greet(null));    // Output: null
```

### OR Operator (||)
```javascript
// Basic usage
const a = true || false;     // true
const b = false || false;    // false
const c = false || true;     // true
const d = true || true;      // true

// Short-circuit evaluation
const x = true || console.log('This won\'t print');
// Returns true (truthy), stops evaluation

// Chaining
const result = 0 || '' || 'default'; // Returns 'default' (first truthy)

// Default values
const name = user?.name || 'Anonymous';
const timeout = config?.timeout || 5000;
```

### NOT Operator (!)
```javascript
// Basic usage
const a = !true;    // false
const b = !false;   // true
const c = !0;       // true
const d = !1;       // false
const e = !'hello'; // false
const f = !'';      // true

// Double NOT (!! ) converts to boolean
const bool1 = !!true;      // true
const bool2 = !!0;         // false
const bool3 = !!'hello';   // true
const bool4 = !!null;      // false

// Negating conditions
const isLoggedIn = false;
if (!isLoggedIn) {
  console.log('Please log in');
}
```

### Nullish Coalescing (??)
```javascript
// Only checks for null or undefined (not 0, '', false)
const a = null ?? 'default';    // 'default'
const b = undefined ?? 'default'; // 'default'
const c = 0 ?? 'default';       // 0 (not falsy check!)
const d = '' ?? 'default';      // '' (not falsy check!)
const e = false ?? 'default';   // false (not falsy check!)

// vs OR operator
const x = 0 || 'default';   // 'default' (0 is falsy)
const y = 0 ?? 'default';   // 0 (only null/undefined trigger default)

// Practical use: preserving falsy values
function processValue(value) {
  return value ?? 'N/A';
}

console.log(processValue(0));      // Output: 0
console.log(processValue(''));     // Output: ''
console.log(processValue(null));   // Output: N/A
```

### Combining Operators
```javascript
// AND and OR together
const age = 25;
const hasID = true;

const canEnter = age >= 21 && hasID;
console.log(canEnter); // Output: true

// Complex conditions
const isWeekend = true;
const isHoliday = false;
const canRelax = isWeekend || isHoliday;
console.log(canRelax); // Output: true

// Nested conditions
const user = { admin: true, editor: false };
const canDelete = user.admin || user.editor;
console.log(canDelete); // Output: true
```

### Short-Circuit Evaluation
```javascript
// AND: returns first falsy or last value
console.log(1 && 2);           // 2
console.log(0 && 2);           // 0
console.log(null && 'hello');  // null

// OR: returns first truthy or last value
console.log(0 || 2);           // 2
console.log('' || 'hello');    // 'hello'
console.log(0 || false);       // false

// Practical: conditional function calls
const user = { name: 'Alice', isAdmin: true };
user.isAdmin && deleteUser();  // Only calls if admin
user.name || createDefault();  // Only calls if no name
```

### Logical Assignment
```javascript
// AND assignment (&&=)
let a = 1;
a &&= 2;
console.log(a); // 2

// OR assignment (||=)
let b = null;
b ||= 'default';
console.log(b); // 'default'

// Nullish assignment (??=)
let c = 0;
c ??= 'default';
console.log(c); // 0 (0 is not null/undefined)
```

### Real-World Examples
```javascript
// Guard clauses
function processUser(user) {
  if (!user || !user.name) {
    return 'Invalid user';
  }
  return `Processing ${user.name}`;
}

// Default values with fallback
const config = {
  host: 'localhost',
  port: undefined
};

const host = config.host ?? '0.0.0.0';  // 'localhost'
const port = config.port ?? 3000;        // 3000

// Conditional rendering (React pattern)
const greeting = isLoggedIn && <Welcome name={userName} />;
const fallback = !isLoggedIn || <LoginPrompt />;

// Null-safe property access
const street = user?.address?.street ?? 'No address';
```

---

## Common Use Cases

| Use Case | Description |
|----------|-------------|
| **Default Values** | Provide fallbacks with `||` or `??` |
| **Guard Clauses** | Prevent errors with `&&` |
| **Conditional Logic** | Combine conditions in if statements |
| **Short-Circuit** | Stop evaluation early |
| **Boolean Conversion** | Use `!!` to convert to boolean |
| **Conditional Assignment** | Assign values based on conditions |

---

## Common Mistakes

### 1. Confusing || and ??
```javascript
// WRONG - Using || for default values
const count = inputCount || 10;
// If inputCount is 0, count becomes 10 (wrong!)

// CORRECT - Using ?? for default values
const count = inputCount ?? 10;
// If inputCount is 0, count stays 0
```

### 2. Wrong Operator Precedence
```javascript
// WRONG - && has higher precedence than ||
const result = a && b || c;
// Interpreted as: (a && b) || c

// CORRECT - Use parentheses for clarity
const result = (a && b) || c;
const result2 = a && (b || c);
```

### 3. Forgetting Short-Circuit Behavior
```javascript
// This doesn't always work as expected
const result = condition && functionCall();
// If condition is falsy, functionCall() is never executed
// This may cause unexpected behavior if functionCall() has side effects
```

---

## Quick Revision Summary

- **AND (`&&`)**: Returns first falsy value or last value; short-circuits on falsy
- **OR (`||`)**: Returns first truthy value or last value; short-circuits on truthy
- **NOT (`!`)**: Inverts boolean value; `!!` converts to boolean
- **Nullish Coalescing (`??`)**: Returns right side only if left is `null` or `undefined`
- `||` treats `0`, `''`, `false` as falsy; `??` does not
- Use parentheses for complex combinations to avoid precedence issues
- Short-circuit evaluation can prevent function calls

---

## Related Topics

- [[function]] - Functions using logical operators
- [[let]] - Variables with conditional values
- [[JavaScript]] - JavaScript language overview
- [[Function-Scope-and-Closures]] - Scope in conditional logic
- [[Local-Storage]] - Using logical operators for defaults
