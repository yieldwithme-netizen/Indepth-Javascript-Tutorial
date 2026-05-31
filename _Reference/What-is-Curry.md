# Currying

Currying is a technique where a function with multiple arguments is transformed into a sequence of functions, each taking a single argument.

## Basic Currying

```javascript
function add(a) {
  return function(b) {
    return a + b;
  };
}

const add5 = add(5);
console.log(add5(3));   // 8
console.log(add5(10));  // 15
```

## Generic Curry Function

```javascript
function curry(fn) {
  return function curried(...args) {
    if (args.length >= fn.length) {
      return fn.apply(this, args);
    }
    return function(...args2) {
      return curried.apply(this, args.concat(args2));
    };
  };
}

const curriedAdd = curry((a, b, c) => a + b + c);

console.log(curriedAdd(1)(2)(3));     // 6
console.log(curriedAdd(1, 2)(3));     // 6
console.log(curriedAdd(1)(2, 3));     // 6
console.log(curriedAdd(1, 2, 3));     // 6
```

## Practical Examples

### Formatting Functions

```javascript
const formatCurrency = curry((symbol, amount) => `${symbol}${amount.toFixed(2)}`);

const formatUSD = formatCurrency('$');
const formatEUR = formatCurrency('€');

console.log(formatUSD(10.5));  // $10.50
console.log(formatEUR(25.3));  // €25.30
```

### Array Operations

```javascript
const map = curry((fn, arr) => arr.map(fn));
const filter = curry((fn, arr) => arr.filter(fn));
const prop = curry((key, obj) => obj[key]);

const getName = prop('name');
const adults = filter(user => user.age >= 18);
const getNames = map(getName);

const users = [
  { name: 'Alice', age: 25 },
  { name: 'Bob', age: 15 },
  { name: 'Charlie', age: 30 }
];

const adultNames = getNames(adults(users));
console.log(adultNames); // ['Alice', 'Charlie']
```

## Partial Application vs Currying

```javascript
// Partial Application
function partial(fn, ...args) {
  return function(...args2) {
    return fn(...args, ...args2);
  };
}

const add5Partial = partial(add, 5);

// Currying
const add5Curried = curry(add);

// Both produce the same result
console.log(add5Partial(3));  // 8
console.log(add5Curried(3));  // 8
```

## Common Use Cases

- Creating reusable utility functions
- Function composition
- Creating specialized functions from general ones
- Building middleware pipelines
- Creating configuration functions

## Common Mistakes

- Currying functions that don't need it
- Over-complicating simple function calls
- Not handling `this` context properly
- Creating curried functions with too many arguments
- Forgetting to return functions at each step

## Related Topics

- [[Closures]]
- [[Higher-Order Functions]]
- [[Function Composition]]
- [[Partial Application]]
- [[Arrow Functions]]

## Quick Revision

- Currying transforms multi-argument functions into single-argument chains
- Each curried function returns another function until all args are provided
- Useful for creating specialized functions
- Enables function composition and reuse
- `curry(fn)` converts any function into a curried version
