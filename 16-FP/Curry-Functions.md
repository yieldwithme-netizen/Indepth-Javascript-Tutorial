# How to Curry Functions

This guide covers practical techniques for creating curried functions in JavaScript, from manual implementations to utility libraries.

## Manual Currying

### Two-Argument Function
```javascript
function curry(fn) {
  return function(a) {
    return function(b) {
      return fn(a, b);
    };
  };
}

const add = (a, b) => a + b;
const curriedAdd = curry(add);

curriedAdd(2)(3); // 5
```

### Generic Curry (Any Argument Count)
```javascript
function curry(fn) {
  return function curried(...args) {
    if (args.length >= fn.length) {
      return fn(...args);
    }
    return (...args2) => curried(...args, ...args2);
  };
}

const sum = (a, b, c) => a + b + c;
const curriedSum = curry(sum);

curriedSum(1)(2)(3);   // 6
curriedSum(1, 2)(3);   // 6
curriedSum(1)(2, 3);   // 6
```

## Arrow Function Currying

```javascript
const add = a => b => a + b;
const multiply = a => b => a * b;

const add5 = add(5);
[1, 2, 3].map(add5); // [6, 7, 8]

const double = multiply(2);
[1, 2, 3].map(double); // [2, 4, 6]
```

## Practical Examples

### Logger
```javascript
const log = level => message => timestamp => {
  console.log(`[${timestamp}] [${level}] ${message}`);
};

const errorLog = log("ERROR");
const logWithTime = errorLog("Disk full");
logWithTime(new Date().toISOString());
```

### Validator Factory
```javascript
const minLength = min => value => value.length >= min;
const maxLength = max => value => value.length <= max;

const isValidUsername = username => {
  const checks = [minLength(3), maxLength(20)];
  return checks.every(check => check(username));
};

isValidUsername("ab");    // false
isValidUsername("alice"); // true
```

### URL Builder
```javascript
const buildUrl = base => path => params => {
  const query = new URLSearchParams(params).toString();
  return `${base}/${path}?${query}`;
};

const api = buildUrl("https://api.example.com");
const usersUrl = api("users");
usersUrl({ page: 1, limit: 10 });
// "https://api.example.com/users?page=1&limit=10"
```

## Using Lodash _.curry

```javascript
import _ from "lodash";

const add = (a, b) => a + b;
const curriedAdd = _.curry(add);

curriedAdd(1, 2); // 3
curriedAdd(1)(2); // 3
```

## Common Mistakes

- Currying functions that rarely need partial application
- Not using the generic curry helper for variadic functions
- Making deeply nested curried functions that are hard to read

## Quick Revision

- Generic curry accepts `fn.length` arguments before executing
- Arrow functions naturally support currying syntax
- Use curry for creating reusable, specialized functions
- Libraries like Lodash provide battle-tested curry implementations

## Related Topics

- [[What-is-Currying]]
- [[What-is-Partial]]
- [[What-is-Composition]]
- [[Function-Scope-and-Closures]]
