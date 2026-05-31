# Symbol.iterator

## Definition

`Symbol.iterator` is a built-in symbol that defines the default iteration protocol for an object. When an object implements `Symbol.iterator`, it becomes **iterable** and can be used with `for...of` loops, spread operator (`...`), destructuring, and other constructs that expect iterables.

---

## How Iteration Works

An iterable object must implement the **iterator protocol**:

1. Has a `[Symbol.iterator]` method
2. Returns an object with a `next()` method
3. `next()` returns `{ value: <any>, done: <boolean> }`

```javascript
const iterable = {
  [Symbol.iterator]() {
    let count = 0;
    const max = 5;

    return {
      next() {
        count++;
        if (count <= max) {
          return { value: count, done: false };
        }
        return { value: undefined, done: true };
      }
    };
  }
};

// Now iterable works with for...of
for (const num of iterable) {
  console.log(num); // 1, 2, 3, 4, 5
}

// Works with spread
const arr = [...iterable]; // [1, 2, 3, 4, 5]
```

---

## Built-in Iterables

JavaScript has built-in iterables:

```javascript
// Strings
const str = "hello";
for (const char of str) {
  console.log(char); // h, e, l, l, o
}

// Arrays
const arr = [1, 2, 3];
for (const val of arr) {
  console.log(val); // 1, 2, 3
}

// Maps
const map = new Map([["a", 1], ["b", 2]]);
for (const [key, val] of map) {
  console.log(key, val);
}

// Sets
const set = new Set([1, 2, 3]);
for (const val of set) {
  console.log(val);
}

// TypedArrays
const typed = new Uint8Array([1, 2, 3]);
for (const val of typed) {
  console.log(val);
}

// arguments object
function test() {
  for (const arg of arguments) {
    console.log(arg);
  }
}
```

---

## Creating Custom Iterables

### Range Iterator

```javascript
class Range {
  constructor(start, end, step = 1) {
    this.start = start;
    this.end = end;
    this.step = step;
  }

  [Symbol.iterator]() {
    let current = this.start;
    const end = this.end;
    const step = this.step;

    return {
      next() {
        if (current <= end) {
          const value = current;
          current += step;
          return { value, done: false };
        }
        return { value: undefined, done: true };
      }
    };
  }
}

// Usage
for (const num of new Range(1, 10, 2)) {
  console.log(num); // 1, 3, 5, 7, 9
}

const arr = [...new Range(1, 5)]; // [1, 2, 3, 4, 5]
```

### Repeater

```javascript
class Repeater {
  constructor(value, times) {
    this.value = value;
    this.times = times;
  }

  [Symbol.iterator]() {
    let count = 0;
    const { value, times } = this;

    return {
      next() {
        if (count < times) {
          count++;
          return { value, done: false };
        }
        return { value: undefined, done: true };
      }
    };
  }
}

[...new Repeater("hello", 3)]; // ["hello", "hello", "hello"]
```

### Tree Traversal

```javascript
class TreeNode {
  constructor(value, left = null, right = null) {
    this.value = value;
    this.left = left;
    this.right = right;
  }

  // In-order traversal
  [Symbol.iterator]() {
    const results = [];

    function traverse(node) {
      if (node === null) return;
      traverse(node.left);
      results.push(node.value);
      traverse(node.right);
    }

    traverse(this);
    return results[Symbol.iterator]();
  }
}

const tree = new TreeNode(
  1,
  new TreeNode(2, new TreeNode(4), new TreeNode(5)),
  new TreeNode(3)
);

console.log([...tree]); // [4, 2, 5, 1, 3]
```

---

## Iterator vs Iterable

```javascript
// Iterable: has [Symbol.iterator] method
// Iterator: has next() method

// An iterable returns an iterator
const iterable = {
  [Symbol.iterator]() {
    // This returns an iterator
    return {
      current: 1,
      last: 5,
      next() {
        if (this.current <= this.last) {
          return { value: this.current++, done: false };
        }
        return { done: true };
      }
    };
  }
};
```

---

## Common Use Cases

### Lazy Evaluation

```javascript
class Fibonacci {
  [Symbol.iterator]() {
    let a = 0, b = 1;

    return {
      next() {
        const value = a;
        [a, b] = [b, a + b];
        return { value, done: false }; // Infinite!
      }
    };
  }
}

// Take first 10 fibonacci numbers
const fib = new Fibonacci();
const first10 = [];
for (const num of fib) {
  if (first10.length >= 10) break;
  first10.push(num);
}
console.log(first10); // [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
```

### Custom Collection

```javascript
class Stack {
  constructor() {
    this.items = [];
  }

  push(item) {
    this.items.push(item);
  }

  pop() {
    return this.items.pop();
  }

  // LIFO iteration
  [Symbol.iterator]() {
    const items = [...this.items].reverse();
    let index = 0;

    return {
      next() {
        if (index < items.length) {
          return { value: items[index++], done: false };
        }
        return { value: undefined, done: true };
      }
    };
  }
}

const stack = new Stack();
stack.push(1);
stack.push(2);
stack.push(3);

console.log([...stack]); // [3, 2, 1] (LIFO order)
```

---

## Common Mistakes

### Mistake 1: Forgetting to Return Iterator

```javascript
// Wrong: returns nothing
const obj = {
  [Symbol.iterator]() {
    // Missing return!
  }
};

[...obj]; // TypeError
```

### Mistake 2: Not Implementing done Properly

```javascript
// Wrong: infinite loop
const obj = {
  [Symbol.iterator]() {
    return {
      next() {
        return { value: 1 }; // Missing done property
      }
    };
  }
};

// Correct
const obj2 = {
  [Symbol.iterator]() {
    let count = 0;
    return {
      next() {
        count++;
        if (count <= 3) {
          return { value: count, done: false };
        }
        return { value: undefined, done: true };
      }
    };
  }
};
```

### Mistake 3: Confusing Iterator and Iterable

```javascript
// Wrong: iterator is not iterable
const iterator = {
  next() {
    return { value: 1, done: false };
  }
};

// for (const val of iterator) {} // TypeError!

// Correct: return iterator from iterable
const iterable = {
  [Symbol.iterator]() {
    return {
      next() {
        return { value: 1, done: false };
      }
    };
  }
};
```

---

## Quick Revision Summary

| Concept | Description |
|---------|-------------|
| `Symbol.iterator` | Symbol method that returns iterator |
| Iterator | Object with `next()` returning `{ value, done }` |
| Iterable | Object with `[Symbol.iterator]` method |
| Built-in iterables | String, Array, Map, Set, TypedArray |
| `for...of` | Uses iteration protocol |
| Spread (`...`) | Uses iteration protocol |

---

## Related Topics

- [[Array]] - Arrays are iterables
- [[loop]] - `for...of` uses iteration protocol
- [[Object]] - Objects are not iterable by default
- [[Promise]] - Async iteration with `for await...of`
