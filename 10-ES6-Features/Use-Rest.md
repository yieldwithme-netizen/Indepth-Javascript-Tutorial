# How to Use Rest Parameters

## Definition
Rest parameters (`...`) allow a function to accept an indefinite number of arguments as an array. They collect remaining arguments into a single array, replacing the need for the `arguments` object.

## Syntax

```javascript
function sum(...numbers) {
  return numbers.reduce((total, n) => total + n, 0);
}

sum(1, 2, 3);       // 6
sum(10, 20, 30, 40); // 100
```

## Rest vs Arguments Object

```javascript
// Old way using arguments object
function oldSum() {
  let total = 0;
  for (let i = 0; i < arguments.length; i++) {
    total += arguments[i];
  }
  return total;
}

// Modern way using rest parameters
function newSum(...nums) {
  return nums.reduce((acc, n) => acc + n, 0);
}
```

## Rest Parameters with Other Parameters

```javascript
function multiply(multiplier, ...numbers) {
  return numbers.map(n => n * multiplier);
}

multiply(2, 1, 2, 3);  // [2, 4, 6]
multiply(5, 10, 20);   // [50, 100]
```

## Rest Parameters in Arrow Functions

```javascript
const subtract = (a, ...rest) => {
  return rest.reduce((result, n) => result - n, a);
};

subtract(10, 2, 3);  // 5
```

## Destructuring with Rest

```javascript
const [first, second, ...remaining] = [1, 2, 3, 4, 5];
console.log(first);      // 1
console.log(second);     // 2
console.log(remaining);  // [3, 4, 5]

const { name, age, ...address } = {
  name: "John",
  age: 30,
  city: "NYC",
  zip: "10001"
};
console.log(address);  // { city: "NYC", zip: "10001" }
```

## Common Use Cases

### Forwarding Arguments

```javascript
function log(...args) {
  console.log(new Date().toISOString(), ...args);
}

log("Server started", "on port 3000");
// 2026-05-30T12:00:00.000Z Server started on port 3000
```

### Flexible Function Signatures

```javascript
function createArray(defaultValue, length) {
  return Array.from({ length }, () => defaultValue);
}

function mergeArrays(target, ...sources) {
  return sources.reduce((merged, source) => [...merged, ...source], target);
}
```

## Common Mistakes

```javascript
// Wrong: Rest parameter must be last
function wrong(a, ...rest, b) {}  // SyntaxError

// Correct: Rest parameter must be last
function correct(a, b, ...rest) {}

// Wrong: Using rest in getter
const obj = {
  get ...items() {}  // SyntaxError
};
```

## Quick Revision

- `...args` collects all remaining arguments into an array
- Must be the last parameter in the function
- Replaces the `arguments` object with a proper array
- Works with arrow functions
- Can be used in destructuring patterns

## Related Topics

- [[Use-Destructuring]] - Destructuring assignment syntax
- [[Use-Spread]] - Spread syntax (opposite of rest)
- [[Write-Arrow]] - Arrow function syntax
- [[Rest-Parameters-Spec]] - MDN documentation
