# How to Write Recursive Functions

## Basic Syntax

```javascript
function functionName(parameters) {
    if (baseCase) {
        return value;
    }
    return functionName(modifiedParameters);
}
```

## Examples

```javascript
// Countdown
function countdown(n) {
    if (n <= 0) {
        console.log("Done!");
        return;
    }
    console.log(n);
    countdown(n - 1);
}

// Factorial
function factorial(n) {
    if (n <= 1) return 1;
    return n * factorial(n - 1);
}

// Fibonacci
function fibonacci(n) {
    if (n <= 1) return n;
    return fibonacci(n - 1) + fibonacci(n - 2);
}

// Sum array
function sum(arr) {
    if (arr.length === 0) return 0;
    return arr[0] + sum(arr.slice(1));
}

// Power
function power(base, exp) {
    if (exp === 0) return 1;
    return base * power(base, exp - 1);
}
```

## Two Parts

```javascript
function factorial(n) {
    // Base case (stops recursion)
    if (n <= 1) {
        return 1;
    }
    
    // Recursive case (calls itself)
    return n * factorial(n - 1);
}
```

## Quick Revision

- Recursion = function calling itself
- Must have base case (stops recursion)
- Must have recursive case (calls itself)
- Can cause stack overflow if too deep
- Use for: trees, math, self-similar problems

---

## Related Topics

- [[What-is-Recursion]] - [[What-is-Recursion|Recursion]] overview
- [[What-is-Function]] - [[What-is-Function|Functions]]
- [[Write-Recursion]] - [[Write-Recursion|Writing recursion]]
- [[What-is-Callback]] - [[What-is-Callback|Callbacks]]
- [[What-is-Closure]] - [[What-is-Closure|Closures]]
