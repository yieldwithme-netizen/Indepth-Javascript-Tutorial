# Rest Parameters

## Definition

Rest parameters (`...`) collect **multiple arguments into an array**.

## Basic Syntax

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
// Old way (not an array)
function oldSum() {
    return Array.from(arguments).reduce((a, b) => a + b, 0);
}

// Modern way (is an array)
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

- [[What-is-RestParam]] - [[What-is-RestParam|Rest parameters]] overview
- [[Use-Rest]] - [[Use-Rest|Using rest]]
- [[What-is-Spread]] - [[What-is-Spread|Spread operator]]
- [[Rest-Parameters]] - [[Rest-Parameters|Rest parameters]]
