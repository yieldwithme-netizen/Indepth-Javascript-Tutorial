# What is a Higher-Order Function (HOF)

## Definition

A **higher-order function** is a function that either takes one or more functions as arguments, or returns a function as its result. This is a core concept in functional programming.

## Built-in Higher-Order Functions

### Array Methods

```javascript
const numbers = [1, 2, 3, 4, 5];

// map: Transform each element
const doubled = numbers.map((n) => n * 2);
console.log(doubled); // [2, 4, 6, 8, 10]

// filter: Select elements
const evens = numbers.filter((n) => n % 2 === 0);
console.log(evens); // [2, 4]

// reduce: Accumulate values
const sum = numbers.reduce((acc, n) => acc + n, 0);
console.log(sum); // 15

// forEach: Execute side effect
numbers.forEach((n) => console.log(n));

// find: Get first match
const found = numbers.find((n) => n > 3);
console.log(found); // 4

// every / some: Test conditions
const allPositive = numbers.every((n) => n > 0);
const hasEven = numbers.some((n) => n % 2 === 0);
```

## Creating Custom HOFs

### Function That Takes a Function

```javascript
function repeat(n, action) {
  for (let i = 0; i < n; i++) {
    action(i);
  }
}

repeat(3, (i) => console.log(`Iteration ${i}`));
// Iteration 0, Iteration 1, Iteration 2
```

### Function That Returns a Function

```javascript
function multiply(factor) {
  return (number) => number * factor;
}

const double = multiply(2);
const triple = multiply(3);

console.log(double(5)); // 10
console.log(triple(5)); // 15
```

### Function Composition

```javascript
const compose = (...fns) => (x) => fns.reduceRight((acc, fn) => fn(acc), x);

const add1 = (x) => x + 1;
const double2 = (x) => x * 2;
const subtract3 = (x) => x - 3;

const transform = compose(subtract3, double2, add1);
console.log(transform(5)); // (5+1)*2-3 = 9
```

## Common Use Cases

- **Data transformation**: `map`, `filter`, `reduce`
- **Event handling**: Callbacks for event listeners
- **Decorators**: Enhance function behavior
- **Currying**: Create specialized functions from general ones

## Common Mistakes

```javascript
// Mistake: Not returning in a callback
const result = [1, 2, 3].map((n) => {
  n * 2; // Missing return!
});
console.log(result); // [undefined, undefined, undefined]

// Fix: Explicit return or concise body
const result2 = [1, 2, 3].map((n) => n * 2);
console.log(result2); // [2, 4, 6]
```

## Related Topics

- [[Create-Closure]]
- [[Prototype-Chain]]
- [[Use-Private]]

## Quick Revision

| Concept | Description |
|---------|-------------|
| HOF | Function taking/returning a function |
| `map` | Transform array elements |
| `filter` | Select matching elements |
| `reduce` | Accumulate to single value |
| Composition | Combine functions into one |
