# Function Composition

## Definition

Function composition **combines multiple functions** into one.

## Basic Syntax

```javascript
const compose = (f, g) => (x) => f(g(x));

const add1 = (x) => x + 1;
const double = (x) => x * 2;

const add1ThenDouble = compose(double, add1);
add1ThenDouble(3); // 8 (3+1=4, 4*2=8)
```

## Pipe (Left to Right)

```javascript
const pipe = (...fns) => (x) => fns.reduce((v, f) => f(v), x);

const transform = pipe(
    (x) => x + 1,
    (x) => x * 2,
    (x) => x - 3
);

transform(5); // 9 ((5+1)*2-3)
```

## Quick Revision

- Compose: combine functions
- Right to left: `compose(f, g)(x)` = `f(g(x))`
- Left to right: `pipe(f, g)(x)` = `g(f(x))`
- Use for: data transformation

---

## Related Topics

- [[What-is-Composition]] - [[What-is-Composition|Composition]]
- [[Compose-Functions]] - [[Compose-Functions|Composing functions]]
- [[What-is-FP]] - [[What-is-FP|Functional programming]]
- [[What-is-HOF]] - [[What-is-HOF|Higher-order functions]]
