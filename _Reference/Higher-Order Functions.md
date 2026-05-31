# Higher-Order Functions

## Definition

Higher-order functions are functions that **take functions as arguments** or **return functions**.

## Taking Functions as Arguments

```javascript
// Array methods are higher-order functions
const numbers = [1, 2, 3, 4, 5];

numbers.map(x => x * 2);      // Takes callback
numbers.filter(x => x > 2);   // Takes callback
numbers.reduce((a, b) => a + b, 0); // Takes callback
```

## Returning Functions

```javascript
function createMultiplier(factor) {
    return (n) => n * factor;
}

const double = createMultiplier(2);
const triple = createMultiplier(3);

double(5); // 10
triple(5); // 15
```

## Common Examples

```javascript
// forEach
[1, 2, 3].forEach(x => console.log(x));

// map
[1, 2, 3].map(x => x * 2);

// filter
[1, 2, 3].filter(x => x > 1);

// reduce
[1, 2, 3].reduce((a, b) => a + b, 0);

// every
[1, 2, 3].every(x => x > 0); // true

// some
[1, 2, 3].some(x => x > 2); // true
```

## Quick Revision

- Higher-order: takes/returns functions
- Array methods are higher-order
- Use for: composition, abstraction
- Enable functional programming

---

## Related Topics

- [[What-is-HOF]] - [[What-is-HOF|Higher-order functions]]
- [[Higher-Order Functions]] - [[Higher-Order Functions|Higher-order functions]]
- [[What-is-Function]] - [[What-is-Function|Functions]]
- [[What-is-FP]] - [[What-is-FP|Functional programming]]
