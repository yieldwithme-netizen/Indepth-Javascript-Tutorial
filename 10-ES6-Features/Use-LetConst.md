# Use Let and Const

## Definition

`let` and `const` are block-scoped variable declarations introduced in ES6. `let` allows reassignment, while `const` cannot be reassigned after declaration.

## Code Examples

### Basic Let

```javascript
let count = 0;
count = 1; // Allowed
count = count + 1; // Allowed
console.log(count); // 2
```

### Basic Const

```javascript
const PI = 3.14159;
// PI = 3; // Error: Assignment to constant variable
console.log(PI); // 3.14159
```

### Block Scope

```javascript
if (true) {
  let x = 10;
  const y = 20;
  var z = 30;
}

// console.log(x); // ReferenceError
// console.log(y); // ReferenceError
console.log(z); // 30 (var is function scoped)
```

### Loop Scope

```javascript
// var - function scoped
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100);
}
// Output: 3, 3, 3

// let - block scoped
for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100);
}
// Output: 0, 1, 2
```

### Const with Objects

```javascript
const person = {
  name: 'John',
  age: 30
};

person.age = 31; // Allowed (modifying property)
// person = {}; // Error (reassigning variable)
```

### Const with Arrays

```javascript
const numbers = [1, 2, 3];

numbers.push(4); // Allowed (modifying array)
// numbers = []; // Error (reassigning variable)
```

### Temporal Dead Zone

```javascript
// console.log(x); // ReferenceError
let x = 5;

// console.log(y); // ReferenceError
const y = 10;
```

### When to Use Each

```javascript
// Use const by default
const API_URL = 'https://api.example.com';
const MAX_SIZE = 100;

// Use let when reassignment is needed
let counter = 0;
counter++;

let isValid = false;
isValid = true;

// Avoid var
// var is function scoped, not block scoped
```

## When to Use Each

| Keyword | Use When |
|---------|----------|
| `const` | Value won't change (default choice) |
| `let` | Value will be reassigned |
| `var` | Avoid - function scoped |

## Common Use Cases

1. **Constants** - API URLs, configuration values
2. **Loop counters** - `let i = 0`
3. **Temporary variables** - `let temp = value`
4. **State flags** - `let isActive = true`

## Common Mistakes

```javascript
// Wrong: Using var
var name = 'John'; // Function scoped

// Correct: Use const or let
const name = 'John';

// Wrong: Not using const by default
let API_KEY = '123'; // Will never change

// Correct
const API_KEY = '123';

// Wrong: Trying to reassign const
const user = {};
user = {}; // Error

// Correct: Modify properties
const user = {};
user.name = 'John'; // Allowed
```

## Related Topics

- [[Destructure-Objects]]
- [[Use-Spread]]
- [[Set-Defaults]]

## Quick Revision

| Keyword | Scope | Reassignable | Use Case |
|---------|-------|--------------|----------|
| `const` | Block | No | Fixed values |
| `let` | Block | Yes | Changing values |
| `var` | Function | Yes | Avoid |
