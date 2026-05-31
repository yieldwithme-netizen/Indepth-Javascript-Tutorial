# Recursion in JavaScript

## Definition

**Recursion** is a technique where a function calls itself to solve a problem by breaking it down into smaller, similar subproblems. Each recursive call moves closer to a **base case** that stops the recursion. Recursion is elegant for problems like tree traversal, mathematical sequences, and divide-and-conquer algorithms.

---

## Basic Recursion

```javascript
// Countdown example
function countdown(n) {
  // Base case: stop when n <= 0
  if (n <= 0) {
    console.log("Done!");
    return;
  }
  
  console.log(n);
  // Recursive call with smaller value
  countdown(n - 1);
}

countdown(5); // 5, 4, 3, 2, 1, Done!

// Factorial: n! = n × (n-1)!
function factorial(n) {
  // Base case
  if (n <= 1) return 1;
  // Recursive case
  return n * factorial(n - 1);
}

factorial(5); // 5 × 4 × 3 × 2 × 1 = 120
```

---

## Components of Recursion

```javascript
function recursiveFunction(input) {
  // 1. Base case: stops recursion
  if (input meets condition) {
    return result;
  }
  
  // 2. Recursive case: calls itself
  return recursiveFunction(smallerInput);
}

// Example: Sum of array
function sumArray(arr, index = 0) {
  // Base case: reached end of array
  if (index >= arr.length) return 0;
  
  // Recursive case: current element + sum of rest
  return arr[index] + sumArray(arr, index + 1);
}

sumArray([1, 2, 3, 4, 5]); // 15
```

---

## Common Recursive Patterns

### Linear Recursion

```javascript
// Factorial
function factorial(n) {
  if (n <= 1) return 1;
  return n * factorial(n - 1);
}

// Fibonacci (inefficient without memoization)
function fibonacci(n) {
  if (n <= 1) return n;
  return fibonacci(n - 1) + fibonacci(n - 2);
}

// Power
function power(base, exp) {
  if (exp === 0) return 1;
  return base * power(base, exp - 1);
}
```

### Tree Recursion

```javascript
// Binary tree sum
function sumTree(node) {
  if (!node) return 0;
  return node.value + sumTree(node.left) + sumTree(node.right);
}

// Tree depth
function depth(node) {
  if (!node) return 0;
  return 1 + Math.max(depth(node.left), depth(node.right));
}
```

### Divide and Conquer

```javascript
// Binary search
function binarySearch(arr, target, low = 0, high = arr.length - 1) {
  if (low > high) return -1;
  
  const mid = Math.floor((low + high) / 2);
  
  if (arr[mid] === target) return mid;
  if (arr[mid] < target) return binarySearch(arr, target, mid + 1, high);
  return binarySearch(arr, target, low, mid - 1);
}

// Merge sort
function mergeSort(arr) {
  if (arr.length <= 1) return arr;
  
  const mid = Math.floor(arr.length / 2);
  const left = mergeSort(arr.slice(0, mid));
  const right = mergeSort(arr.slice(mid));
  
  return merge(left, right);
}
```

### Tail Recursion

```javascript
// Tail recursive factorial (can be optimized)
function factorialTail(n, acc = 1) {
  if (n <= 1) return acc;
  return factorialTail(n - 1, n * acc); // Last call is recursive
}

// Tail recursive sum
function sumTail(arr, index = 0, acc = 0) {
  if (index >= arr.length) return acc;
  return sumTail(arr, index + 1, acc + arr[index]);
}
```

---

## Common Use Cases

### DOM Traversal

```javascript
// Traverse all child nodes
function traverseDOM(node, callback) {
  callback(node);
  
  for (const child of node.children) {
    traverseDOM(child, callback);
  }
}

// Count elements
function countElements(element) {
  let count = 1;
  for (const child of element.children) {
    count += countElements(child);
  }
  return count;
}
```

### Deep Cloning

```javascript
function deepClone(obj) {
  // Base cases
  if (obj === null || typeof obj !== "object") {
    return obj;
  }
  
  // Handle arrays
  if (Array.isArray(obj)) {
    return obj.map(item => deepClone(item));
  }
  
  // Handle objects
  const clone = {};
  for (const key in obj) {
    if (obj.hasOwnProperty(key)) {
      clone[key] = deepClone(obj[key]);
    }
  }
  return clone;
}
```

### Flattening Arrays

```javascript
// Recursive flatten
function flatten(arr) {
  const result = [];
  for (const item of arr) {
    if (Array.isArray(item)) {
      result.push(...flatten(item));
    } else {
      result.push(item);
    }
  }
  return result;
}

flatten([1, [2, 3], [4, [5, 6]]]); // [1, 2, 3, 4, 5, 6]
```

### File System Traversal

```javascript
// Traverse directory structure
async function traverseDir(dirPath, callback) {
  const entries = await fs.readdir(dirPath, { withFileTypes: true });
  
  for (const entry of entries) {
    const fullPath = path.join(dirPath, entry.name);
    
    if (entry.isDirectory()) {
      await traverseDir(fullPath, callback);
    } else {
      await callback(fullPath);
    }
  }
}
```

---

## Memoization (Optimization)

```javascript
// Without memoization: O(2^n)
function fibonacci(n) {
  if (n <= 1) return n;
  return fibonacci(n - 1) + fibonacci(n - 2);
}

// With memoization: O(n)
function fibonacciMemo(n, memo = {}) {
  if (n in memo) return memo[n];
  if (n <= 1) return n;
  
  memo[n] = fibonacciMemo(n - 1, memo) + fibonacciMemo(n - 2, memo);
  return memo[n];
}

// Generic memoize function
function memoize(fn) {
  const cache = {};
  return function(...args) {
    const key = JSON.stringify(args);
    if (key in cache) return cache[key];
    cache[key] = fn.apply(this, args);
    return cache[key];
  };
}

const memoizedFib = memoize(fibonacci);
memoizedFib(100); // Fast!
```

---

## Common Mistakes

### Mistake 1: Missing Base Case

```javascript
// Wrong: infinite recursion
function countdown(n) {
  console.log(n);
  countdown(n - 1); // Never stops!
}

// Correct: add base case
function countdown(n) {
  if (n <= 0) return; // Base case!
  console.log(n);
  countdown(n - 1);
}
```

### Mistake 2: Not Changing Toward Base Case

```javascript
// Wrong: doesn't move toward base case
function factorial(n) {
  if (n <= 1) return 1;
  return n * factorial(n); // Same n forever!
}

// Correct: decrement n
function factorial(n) {
  if (n <= 1) return 1;
  return n * factorial(n - 1);
}
```

### Mistake 3: Stack Overflow

```javascript
// Too many recursive calls
function countDown(n) {
  if (n <= 0) return;
  countDown(n - 1);
}

countDown(100000); // RangeError: Maximum call stack size exceeded

// Solution: use iteration or tail call optimization
function countDownIterative(n) {
  while (n > 0) {
    console.log(n);
    n--;
  }
}
```

### Mistake 4: Redundant Calculations

```javascript
// Inefficient Fibonacci
function fib(n) {
  if (n <= 1) return n;
  return fib(n - 1) + fib(n - 2); // Recalculates same values!
}

// fib(5) calculates fib(3) twice, fib(2) three times, etc.

// Solution: memoization or iteration
function fibIterative(n) {
  if (n <= 1) return n;
  let a = 0, b = 1;
  for (let i = 2; i <= n; i++) {
    [a, b] = [b, a + b];
  }
  return b;
}
```

---

## Recursion vs Iteration

```javascript
// Recursive factorial
function factorialRecursive(n) {
  if (n <= 1) return 1;
  return n * factorialRecursive(n - 1);
}

// Iterative factorial
function factorialIterative(n) {
  let result = 1;
  for (let i = 2; i <= n; i++) {
    result *= i;
  }
  return result;
}

// Both produce same result
factorialRecursive(5); // 120
factorialIterative(5); // 120
```

---

## Quick Revision Summary

| Aspect | Description |
|--------|-------------|
| Base case | Stops recursion |
| Recursive case | Calls itself with smaller input |
| Stack | Each call adds to call stack |
| Tail recursion | Recursive call is last operation |
| Memoization | Cache results to avoid recalculation |
| Use cases | Trees, divide-and-conquer, backtracking |

---

## Related Topics

- [[function]] - Function fundamentals
- [[Arrow]] - Arrow functions
- [[Closures]] - Closures in recursion
- [[Higher-Order-Functions]] - Functions as values
- [[Array-Methods]] - Array methods that use recursion
- [[loop]] - Iterative alternatives
- [[Scope]] - Variable scope in recursion