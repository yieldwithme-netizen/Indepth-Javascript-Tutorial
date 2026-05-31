# How to Compose Functions

This guide covers practical implementations of function composition in JavaScript, including both `compose` and `pipe` patterns.

## Basic compose Implementation

```javascript
const compose = (...fns) => x => fns.reduceRight((acc, fn) => fn(acc), x);

// Usage
const add1 = x => x + 1;
const double = x => x * 2;

const add1ThenDouble = compose(double, add1);
add1ThenDouble(3); // (3 + 1) * 2 = 8
```

## Basic pipe Implementation

```javascript
const pipe = (...fns) => x => fns.reduce((acc, fn) => fn(acc), x);

// Usage
const doubleThenAdd1 = pipe(double, add1);
doubleThenAdd1(3); // (3 * 2) + 1 = 7
```

## Multi-Argument Functions

```javascript
const compose = (...fns) =>
  (...args) => fns.reduceRight((acc, fn, i) =>
    i === fns.length - 1 ? fn(...acc) : fn(acc)
  , args);

const add = (a, b) => a + b;
const double = x => x * 2;

const doubleThenAdd = compose(x => add(x, 5), double);
doubleThenAdd(3); // (3 * 2) + 5 = 11
```

## Practical Examples

### String Processing Pipeline
```javascript
const trim = s => s.trim();
const toLower = s => s.toLowerCase();
const splitBy = sep => s => s.split(sep);
const joinBy = sep => arr => arr.join(sep);
const replace = (pattern, replacement) => s => s.replace(pattern, replacement);

const slugify = pipe(
  trim,
  toLower,
  replace(/\s+/g, "-"),
  replace(/[^a-z0-9-]/g, "")
);

slugify("  Hello World!  "); // "hello-world"
```

### Number Processing
```javascript
const clamp = (min, max) => n => Math.min(Math.max(n, min), max);
const round = n => Math.round(n);
const toFixed = digits => n => Number(n.toFixed(digits));

const processPrice = pipe(
  n => n * 1.1,  // add tax
  round,
  toFixed(2)
);

processPrice(19.99); // 22.0
```

### Data Transformation
```javascript
const filter = pred => arr => arr.filter(pred);
const map = fn => arr => arr.map(fn);
const reduce = (fn, init) => arr => arr.reduce(fn, init);

const sumSquaresOfEvens = pipe(
  filter(n => n % 2 === 0),
  map(n => n * n),
  reduce((a, b) => a + b, 0)
);

sumSquaresOfEvens([1, 2, 3, 4, 5]); // 20 (4 + 16)
```

## Composing with Async Functions

```javascript
const composeAsync = (...fns) =>
  async x => {
    let result = x;
    for (const fn of fns) {
      result = await fn(result);
    }
    return result;
  };

const fetchData = url => async () => fetch(url).then(r => r.json());
const extractData = json => json.data;
const filterActive = items => items.filter(i => i.active);

const getActiveUsers = composeAsync(
  filterActive,
  extractData,
  fetchData("/api/users")
);
```

## Common Mistakes

- Mixing sync and async functions in compose
- Composing functions that expect different argument counts
- Over-nesting compositions making debugging hard

## Quick Revision

- `compose` = right-to-left, `pipe` = left-to-right
- Use `reduceRight` for compose, `reduce` for pipe
- Build pipelines for readable data transformation
- Handle async functions with async compose variants

## Related Topics

- [[What-is-Composition]]
- [[What-is-Currying]]
- [[What-is-Partial]]
- [[What-is-Immutability]]
