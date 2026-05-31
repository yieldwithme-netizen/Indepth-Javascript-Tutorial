# Expressions in JavaScript

## Definition

An expression in JavaScript is a valid combination of values, variables, operators, and function calls that evaluates to produce a result. Expressions can be as simple as a single value or as complex as a multi-part calculation.

## Types of Expressions

### 1. Arithmetic Expressions

```javascript
// Basic arithmetic
const sum = 5 + 3;           // 8
const difference = 10 - 4;   // 6
const product = 6 * 7;       // 42
const quotient = 20 / 4;     // 5
const remainder = 17 % 5;    // 2

// Exponentiation
const power = 2 ** 10;       // 1024

// Increment/Decrement
let count = 5;
count++;                     // 6
count--;                     // 5
```

### 2. String Expressions

```javascript
// String concatenation
const greeting = 'Hello' + ' ' + 'World';  // "Hello World"

// Template literals
const name = 'Alice';
const message = `Hello, ${name}!`;  // "Hello, Alice!"

// String methods as expressions
const upper = 'hello'.toUpperCase();  // "HELLO"
const length = 'hello'.length;        // 5
```

### 3. Comparison Expressions

```javascript
// Equality
const isEqual = 5 == 5;        // true (loose)
const isStrictEqual = 5 === 5; // true (strict)
const isNotEqual = 5 != 3;     // true
const isStrictNotEqual = 5 !== '5'; // true

// Relational
const isGreater = 10 > 5;      // true
const isLess = 3 < 7;          // true
const isGreaterOrEqual = 5 >= 5; // true
const isLessOrEqual = 3 <= 4;  // true
```

### 4. Logical Expressions

```javascript
// Logical AND
const andTrue = true && true;   // true
const andFalse = true && false; // false

// Logical OR
const orTrue = true || false;   // true
const orFalse = false || false; // false

// Logical NOT
const notTrue = !true;          // false
const notFalse = !false;        // true

// Complex logical expressions
const age = 25;
const canVote = age >= 18 && age <= 120;  // true
const isAdult = age >= 18 || age < 0;     // true
```

### 5. Assignment Expressions

```javascript
// Basic assignment
let x = 10;

// Compound assignment
x += 5;    // x = x + 5 → 15
x -= 3;    // x = x - 3 → 12
x *= 2;    // x = x * 2 → 24
x /= 4;    // x = x / 4 → 6
x %= 4;    // x = x % 4 → 2
x **= 3;   // x = x ** 3 → 8

// Assignment as expression
let a, b, c;
a = b = c = 10;  // All become 10
```

### 6. Conditional (Ternary) Expressions

```javascript
// Basic ternary
const age = 20;
const status = age >= 18 ? 'Adult' : 'Minor';

// Nested ternary
const score = 85;
const grade = score >= 90 ? 'A' :
              score >= 80 ? 'B' :
              score >= 70 ? 'C' :
              score >= 60 ? 'D' : 'F';

// Ternary with functions
const isLoggedIn = true;
const buttonText = isLoggedIn ? 'Logout' : 'Login';
```

### 7. Function Expressions

```javascript
// Function expression
const add = function(a, b) {
  return a + b;
};

// Arrow function expression
const multiply = (a, b) => a * b;

// Immediately invoked function expression (IIFE)
const result = (function() {
  return 42;
})();

// Named function expression
const factorial = function fact(n) {
  return n <= 1 ? 1 : n * fact(n - 1);
};
```

### 8. Object Expressions

```javascript
// Object literal
const person = {
  name: 'Alice',
  age: 30,
  greet() {
    return `Hello, I'm ${this.name}`;
  }
};

// Object with computed property names
const propName = 'age';
const obj = {
  [propName]: 25
};

// Object spread
const defaults = { color: 'red', size: 'medium' };
const custom = { ...defaults, color: 'blue' };
```

### 9. Array Expressions

```javascript
// Array literal
const numbers = [1, 2, 3, 4, 5];

// Array with expressions
const x = 5;
const arr = [x, x * 2, x + 10]; // [5, 10, 15]

// Array spread
const arr1 = [1, 2];
const arr2 = [3, 4];
const combined = [...arr1, ...arr2]; // [1, 2, 3, 4]

// Array methods returning expressions
const doubled = [1, 2, 3].map(n => n * 2); // [2, 4, 6]
const sum = [1, 2, 3].reduce((a, b) => a + b, 0); // 6
```

### 10. Template Literal Expressions

```javascript
// Basic template literal
const name = 'World';
const greeting = `Hello, ${name}!`;

// Expression in template
const price = 19.99;
const tax = 0.08;
const total = `Total: $${(price * (1 + tax)).toFixed(2)}`;

// Multi-line template literal
const html = `
  <div class="card">
    <h2>${title}</h2>
    <p>${content}</p>
  </div>
`;
```

## Operator Precedence

### 1. Highest to Lowest Precedence

```javascript
// 1. Grouping: ()
const result1 = (2 + 3) * 4; // 20

// 2. Exponentiation: **
const result2 = 2 ** 3 ** 2; // 512 (right-to-left)

// 3. Multiplication/Division/Remainder
const result3 = 6 * 7 / 2; // 21

// 4. Addition/Subtraction
const result4 = 10 - 3 + 2; // 9

// 5. Relational: <, >, <=, >=
const result5 = 5 > 3; // true

// 6. Equality: ==, ===, !=, !==
const result6 = 5 === 5; // true

// 7. Logical AND: &&
const result7 = true && false; // false

// 8. Logical OR: ||
const result8 = true || false; // true

// 9. Ternary: ?:
const result9 = true ? 'yes' : 'no'; // 'yes'

// 10. Assignment: =, +=, -=, etc.
let x = 10;
```

### 2. Common Precedence Gotchas

```javascript
// WRONG: Misunderstanding precedence
const result = 2 + 3 * 4; // 14, not 20

// RIGHT: Use parentheses for clarity
const result = (2 + 3) * 4; // 20

// WRONG: Ternary without parentheses
const value = true ? 'yes' : false ? 'no' : 'maybe';

// RIGHT: Add parentheses
const value = true ? 'yes' : (false ? 'no' : 'maybe');
```

## Side Effects

### 1. Expressions with Side Effects

```javascript
// Assignment has side effect
let x = 5;
x = 10; // Changes x

// Increment has side effect
let count = 0;
count++; // Changes count

// Function call may have side effect
console.log('Hello'); // Prints to console
document.title = 'New Title'; // Changes page title
```

### 2. Pure vs Impure Expressions

```javascript
// Pure expression (no side effects)
const sum = 5 + 3; // Just calculates, doesn't change anything

// Impure expression (has side effects)
let total = 0;
total += 5; // Changes total

// Function with side effect
function addToTotal(value) {
  total += value; // Changes external state
}
```

## Common Use Cases

### 1. Data Transformation

```javascript
// Transform array of users
const users = [
  { name: 'Alice', age: 25 },
  { name: 'Bob', age: 30 },
  { name: 'Charlie', age: 35 }
];

const names = users.map(user => user.name); // ['Alice', 'Bob', 'Charlie']
const adults = users.filter(user => user.age >= 18);
const totalAge = users.reduce((sum, user) => sum + user.age, 0);
```

### 2. Conditional Rendering

```javascript
// React-like conditional rendering
const isLoggedIn = true;
const element = isLoggedIn ? <UserProfile /> : <LoginForm />;

// Template conditional
const message = `
  <div class="notification ${isActive ? 'active' : 'inactive'}">
    ${hasError ? 'Error occurred' : 'All good'}
  </div>
`;
```

### 3. Default Values

```javascript
// Using OR for defaults
const config = {
  color: userColor || 'blue',
  size: userSize || 'medium'
};

// Using nullish coalescing
const port = config.port ?? 3000;

// Using ternary for defaults
const greeting = name ? `Hello, ${name}!` : 'Hello, stranger!';
```

### 4. Short-Circuit Evaluation

```javascript
// AND operator (if first is falsy, returns it)
const value = someValue && someValue.property;

// OR operator (if first is truthy, returns it)
const config = userConfig || defaultConfig;

// Nullish coalescing
const port = config.port ?? 3000;
```

## Common Mistakes

### 1. Confusing = and ==

```javascript
// WRONG: Assignment instead of comparison
if (x = 5) {  // Always true!
  console.log('This runs');
}

// RIGHT: Use === for comparison
if (x === 5) {
  console.log('x is 5');
}
```

### 2. Forgetting Operator Precedence

```javascript
// WRONG: Unexpected result
const result = 2 + 3 * 4; // 14, not 20

// RIGHT: Use parentheses
const result = (2 + 3) * 4; // 20
```

### 3. Using Wrong Equality Operator

```javascript
// WRONG: Loose equality
const result1 = 5 == '5';   // true (type coercion)
const result2 = 0 == false;  // true
const result3 = '' == false; // true

// RIGHT: Strict equality
const result4 = 5 === '5';   // false
const result5 = 0 === false; // false
const result6 = '' === false; // false
```

### 4. Misusing Ternary Operator

```javascript
// WRONG: Complex nested ternary (hard to read)
const result = a ? b ? c : d : e ? f : g;

// RIGHT: Use if/else or switch for complex logic
let result;
if (a) {
  result = b ? c : d;
} else {
  result = e ? f : g;
}
```

## Quick Revision Summary

- Expressions evaluate to produce a value
- Types: arithmetic, string, comparison, logical, assignment, ternary, function, object, array
- Operator precedence determines evaluation order
- Use parentheses for clarity and to avoid bugs
- Prefer `===` over `==` for comparisons
- Side effects change external state; prefer pure expressions
- Ternary operator is great for simple conditionals

## Related Topics

- [[Operators]] - All JavaScript operators
- [[Comparison-Operators]] - Equality and comparison
- [[Logical-Operators]] - AND, OR, NOT
- [[Ternary-Operator]] - Conditional expressions
- [[Arrow-Functions]] - Function expressions
- [[Template-Literals]] - String expressions
- [[Destructuring-Arrays]] - Array expressions
- [[Object-Destructuring]] - Object expressions
