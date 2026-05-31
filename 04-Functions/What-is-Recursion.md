# What is [[Recursion]]?

## Definition

[[Recursion]] is when a [[Function]] **calls itself** to solve a problem.

## Basic Example

```javascript
function countdown(n) {
    if (n <= 0) {
        console.log("Done!");
        return;
    }
    console.log(n);
    countdown(n - 1); // recursive call
}

countdown(5); // 5, 4, 3, 2, 1, Done!
```

## How It Works

```
countdown(5)
  → prints 5, calls countdown(4)
    → prints 4, calls countdown(3)
      → prints 3, calls countdown(2)
        → prints 2, calls countdown(1)
          → prints 1, calls countdown(0)
            → prints "Done!", returns
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

factorial(5); // 120 (5 * 4 * 3 * 2 * 1)
```

## Common [[Recursion]] Problems

```javascript
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

## When to Use

```javascript
// ✅ Use recursion for:
// - Tree traversal
// - Divide and conquer
// - Mathematical sequences
// - Problems with self-similar structure

// ❌ Avoid recursion for:
// - Simple loops (use for/while)
// - Large data (stack overflow risk)
```

## Quick Revision

- [[Recursion]] = [[Function]] calling itself
- Must have base case (stops recursion)
- Must have recursive case (calls itself)
- Can cause stack overflow if too deep
- Use for: trees, math, self-similar problems

---

## Related Topics

- [[What-is-Function]] - Functions
- [[Write-Recursion]] - Writing recursion
- [[What-is-Callback]] - Callbacks
- [[What-is-Closure]] - Closures