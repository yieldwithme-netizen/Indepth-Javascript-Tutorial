# Higher-Order Functions

## Definition

Higher-order functions **take or return functions**.

## Examples

```javascript
// Takes function
[1, 2, 3].map(x => x * 2);

// Returns function
function createMultiplier(factor) {
    return (n) => n * factor;
}
```

## Quick Revision

- Takes/returns functions
- Array methods are higher-order
- Use for composition

---

## Related Topics

- [[What-is-HOF]] - [[What-is-HOF|Higher-order functions]]
- [[higher-order]] - [[higher-order|Higher-order]]
- [[What-is-Function]] - [[What-is-Function|Functions]]
