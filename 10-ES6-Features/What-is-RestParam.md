# What is Rest Parameter?

## Definition

Rest parameters (`...`) collect **multiple arguments into an array**.

## Basic Usage

```javascript
function sum(...numbers) {
    return numbers.reduce((total, num) => total + num, 0);
}

sum(1, 2, 3);    // 6
sum(1, 2, 3, 4); // 10
```

## Rest with Other Parameters

```javascript
function log(level, ...messages) {
    console.log(`[${level}]`, ...messages);
}

log("INFO", "User logged in", "at 10:00");
// [INFO] User logged in at 10:00
```

## Rest vs Arguments

```javascript
// arguments (old way - not an array)
function oldSum() {
    return Array.from(arguments).reduce((a, b) => a + b, 0);
}

// Rest (modern way - is an array)
function newSum(...args) {
    return args.reduce((a, b) => a + b, 0);
}
```

## Quick Revision

- `...param` collects arguments into array
- Must be last parameter
- Replaces `arguments` object
- Works with arrow functions
- Use for: variable number of arguments

---

## Related Topics

- [[What-is-RestParam]] - Rest parameters overview
- [[Use-Rest]] - Using rest
- [[What-is-Spread]] - Spread operator
- [[What-is-Function]] - Functions
- [[What-is-Parameter]] - Parameters
