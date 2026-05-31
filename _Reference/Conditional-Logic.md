# Conditional Logic

## Definition
Conditional logic allows programs to make decisions and execute different code paths based on whether conditions evaluate to true or false. It is a fundamental control flow mechanism in JavaScript.

## If Statement
```javascript
const temperature = 25;

if (temperature > 30) {
  console.log('It is hot outside');
} else if (temperature > 20) {
  console.log('It is nice outside');
} else if (temperature > 10) {
  console.log('It is cool outside');
} else {
  console.log('It is cold outside');
}
```

## Truthy and Falsy Values
```javascript
// Falsy values (evaluate to false)
false, 0, -0, 0n, '', null, undefined, NaN

// Everything else is truthy
'', // empty string is falsy
'hello', // non-empty string is truthy
[], // empty array is truthy
{}, // empty object is truthy
```

```javascript
// Implicit truthy/falsy checks
const name = '';
if (name) {
  console.log('Name exists'); // not executed
}

const count = 0;
if (count) {
  console.log('Count is non-zero'); // not executed
}
```

## Logical Operators
```javascript
// AND (&&) — both must be true
if (age >= 18 && hasID) {
  console.log('Entry allowed');
}

// OR (||) — at least one must be true
if (isAdmin || isOwner) {
  console.log('Access granted');
}

// NOT (!) — inverts the value
if (!isLoggedIn) {
  console.log('Please log in');
}

// Nullish Coalescing (??) — only checks null/undefined
const username = null;
const display = username ?? 'Guest'; // 'Guest'

const score = 0;
const displayScore = score ?? 10; // 0 (not replaced)

// Optional Chaining (?.)
const street = user?.address?.street;
```

## Comparison Operators
```javascript
// Equality
==   // Loose equality (type coercion)
===  // Strict equality (no coercion) — prefer this
!=   // Loose inequality
!==  // Strict inequality — prefer this

// Relational
>, <, >=, <=

// Examples
0 == false    // true (type coercion)
0 === false   // false (different types)
'' == false   // true
'' === false  // false
null == undefined  // true
null === undefined // false
```

## Ternary Operator
```javascript
// condition ? valueIfTrue : valueIfFalse
const age = 20;
const status = age >= 18 ? 'adult' : 'minor';

// Nested ternary (avoid deep nesting)
const level = score > 90 ? 'A'
  : score > 80 ? 'B'
  : score > 70 ? 'C'
  : 'F';

// Practical usage
const greeting = `Hello, ${user ? user.name : 'Guest'}`;
```

## Short-Circuit Evaluation
```javascript
// AND (&&) — returns first falsy or last value
const result = value && doSomething();

// OR (||) — returns first truthy or last value
const config = userConfig || defaultConfig;

// Nullish coalescing (??) — returns first defined value
const port = envPort ?? 3000;
```

## Switch Statement
```javascript
const day = 'Monday';

switch (day) {
  case 'Monday':
  case 'Tuesday':
  case 'Wednesday':
  case 'Thursday':
  case 'Friday':
    console.log('Weekday');
    break;
  case 'Saturday':
  case 'Sunday':
    console.log('Weekend');
    break;
  default:
    console.log('Invalid day');
}
```

## Pattern Matching with Object Lookup
```javascript
// Alternative to switch for simple mappings
const actions = {
  add: (a, b) => a + b,
  subtract: (a, b) => a - b,
  multiply: (a, b) => a * b,
};

const operation = 'add';
const result = actions[operation]?.(5, 3) ?? 0;
console.log(result); // 8
```

## Common Use Cases
- Form validation before submission
- Feature flags and environment-based logic
- Route matching in navigation
- Permission and access control
- State machine transitions

## Common Mistakes
- Using `=` instead of `===` in conditions
- Forgetting `break` in switch statements
- Deeply nested if-else chains (refactor with early returns)
- Using `==` instead of `===` (unintended type coercion)
- Not handling the default/fallback case

## Related Topics
- [[Condition-Statements]]
- [[Logical-Operators]]
- [[Loops]]
- [[Functions]]
- [[Ternary-Operator]]

## Quick Revision
- Prefer `===` and `!==` over `==`/`!=`
- Use `??` instead of `||` when 0 and empty string are valid
- Ternary for simple conditions, if-else for complex logic
- Short-circuit evaluation avoids unnecessary code execution
- Avoid deep nesting; use early returns and guard clauses
