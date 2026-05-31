# What is Composition

Composition is the technique of combining multiple functions to create a new function, where the output of one function becomes the input of the next.

## Definition

Instead of writing deeply nested calls, composition lets you build complex operations from small, reusable functions.

```javascript
// Without composition - nested calls
const result = add10(multiply2(subtract3(x)));

// With composition - pipe left to right
const process = compose(add10, multiply2, subtract3);
const result = process(x);
```

## Two Types of Composition

### Right-to-Left (compose)
```javascript
const compose = (...fns) => x => fns.reduceRight((acc, fn) => fn(acc), x);

const add10 = x => x + 10;
const multiply2 = x => x * 2;
const subtract3 = x => x - 3;

const process = compose(add10, multiply2, subtract3);
process(5); // (5 - 3) * 2 + 10 = 14
```

### Left-to-Right (pipe)
```javascript
const pipe = (...fns) => x => fns.reduce((acc, fn) => fn(acc), x);

const process = pipe(subtract3, multiply2, add10);
process(5); // (5 - 3) * 2 + 10 = 14
```

## Why Composition Matters

- **Reusability**: Small functions combined in different ways
- **Testability**: Each function can be tested independently
- **Readability**: Data flows left-to-right (in pipe)
- **Maintainability**: Easy to add, remove, or reorder steps

## Practical Examples

### Data Transformation
```javascript
const trim = s => s.trim();
const lowercase = s => s.toLowerCase();
const split = sep => s => s.split(sep);
const join = sep => arr => arr.join(sep);

const slugify = pipe(
  trim,
  lowercase,
  split(" "),
  join("-")
);

slugify("  Hello World  "); // "hello-world"
```

### Form Validation
```javascript
const isRequired = val => val !== "";
const minLength = min => val => val.length >= min;
const isEmail = val => /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(val);

const validateUsername = val =>
  isRequired(val) && minLength(3)(val);

const validateEmail = val =>
  isRequired(val) && isEmail(val);
```

### Array Processing
```javascript
const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

const processNumbers = pipe(
  nums => nums.filter(n => n % 2 === 0),
  nums => nums.map(n => n * n),
  nums => nums.reduce((sum, n) => sum + n, 0)
);

processNumbers(numbers); // 220 (4 + 16 + 36 + 64 + 100)
```

## Common Mistakes

- Composing functions with different signatures
- Over-composing making debugging difficult
- Forgetting that compose runs right-to-left

## Quick Revision

- Composition combines functions: output of one feeds into next
- `compose` runs right-to-left, `pipe` runs left-to-right
- Build complex logic from small, pure functions
- Great for data transformation pipelines

## Related Topics

- [[Compose-Functions]]
- [[What-is-Currying]]
- [[What-is-Immutability]]
- [[What-is-Memoization]]
