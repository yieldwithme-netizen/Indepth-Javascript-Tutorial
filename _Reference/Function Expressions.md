# Function Expressions

## Definition

Function expressions are **functions assigned to variables**.

## Basic Syntax

```javascript
const greet = function(name) {
    return `Hello, ${name}!`;
};
```

## Named vs Anonymous

```javascript
// Named (for stack traces)
const greet = function greet(name) {
    return `Hello, ${name}!`;
};

// Anonymous
const greet = function(name) {
    return `Hello, ${name}!`;
};
```

## Quick Revision

- Function expression = function in variable
- NOT hoisted
- Can be named or anonymous
- Often used for callbacks

---

## Related Topics

- [[What-is-Expression]] - [[What-is-Expression|Function expressions]]
- [[Function Expressions]] - [[Function Expressions|Function expressions]]
- [[Declare-Function]] - [[Declare-Function|Function declarations]]
- [[What-is-Function]] - [[What-is-Function|Functions]]
