# How to Create Iterators

## Definition
An iterator is an object that defines a `next()` method producing values one at a time. Iterators implement the iterator protocol and enable custom iteration over data structures.

## Basic Iterator Pattern

```javascript
function createRangeIterator(start, end) {
  let current = start;

  return {
    next() {
      if (current <= end) {
        return { value: current++, done: false };
      }
      return { value: undefined, done: true };
    }
  };
}

const iter = createRangeIterator(1, 5);
console.log(iter.next());  // { value: 1, done: false }
console.log(iter.next());  // { value: 2, done: false }
console.log(iter.next());  // { value: 3, done: false }
console.log(iter.next());  // { value: 4, done: false }
console.log(iter.next());  // { value: 5, done: false }
console.log(iter.next());  // { value: undefined, done: true }
```

## Making Objects Iterable

```javascript
class Countdown {
  constructor(start) {
    this.start = start;
  }

  [Symbol.iterator]() {
    let current = this.start;

    return {
      next() {
        if (current >= 0) {
          return { value: current--, done: false };
        }
        return { value: undefined, done: true };
      }
    };
  }
}

// Usage
const countdown = new Countdown(5);
for (const num of countdown) {
  console.log(num);  // 5, 4, 3, 2, 1, 0
}

// Convert to array
console.log([...countdown]);  // [5, 4, 3, 2, 1, 0]
```

## Iterator with State Management

```javascript
class Fibonacci {
  constructor(limit) {
    this.limit = limit;
  }

  [Symbol.iterator]() {
    let a = 0, b = 1, count = 0;
    const limit = this.limit;

    return {
      next() {
        if (count >= limit) {
          return { value: undefined, done: true };
        }

        const value = a;
        [a, b] = [b, a + b];
        count++;

        return { value, done: false };
      },

      // Optional: reset iterator
      reset() {
        a = 0;
        b = 1;
        count = 0;
      }
    };
  }
}

const fib = new Fibonacci(10);
console.log([...fib]);  // [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
```

## Infinite Iterator

```javascript
function* infiniteCounter(start = 0) {
  let current = start;
  while (true) {
    yield current++;
  }
}

const counter = infiniteCounter(1);
console.log(counter.next().value);  // 1
console.log(counter.next().value);  // 2
console.log(counter.next().value);  // 3
// Can go forever...
```

## Common Use Cases

### Lazy Data Processing

```javascript
class DataStream {
  constructor(data) {
    this.data = data;
  }

  *[Symbol.iterator]() {
    for (const item of this.data) {
      // Transform lazily
      yield item.toUpperCase();
    }
  }
}

const stream = new DataStream(["a", "b", "c"]);
for (const char of stream) {
  console.log(char);  // "A", "B", "C"
}
```

### Paginated Results

```javascript
class PaginatedIterator {
  constructor(fetchPage, totalPages) {
    this.fetchPage = fetchPage;
    this.totalPages = totalPages;
    this.currentPage = 1;
  }

  [Symbol.iterator]() {
    return {
      next: async () => {
        if (this.currentPage > this.totalPages) {
          return { value: undefined, done: true };
        }

        const data = await this.fetchPage(this.currentPage);
        this.currentPage++;

        return { value: data, done: false };
      }
    };
  }
}
```

### Tree Traversal

```javascript
class TreeNode {
  constructor(value, children = []) {
    this.value = value;
    this.children = children;
  }

  *[Symbol.iterator]() {
    yield this.value;
    for (const child of this.children) {
      yield* child;
    }
  }
}

const tree = new TreeNode(1, [
  new TreeNode(2, [
    new TreeNode(4),
    new TreeNode(5)
  ]),
  new TreeNode(3)
]);

console.log([...tree]);  // [1, 2, 4, 5, 3]
```

## Common Mistakes

```javascript
// Wrong: Forgetting to return an iterator from Symbol.iterator
class BadIterable {
  [Symbol.iterator]() {
    // Returns value, not iterator object
    return { value: 1, done: false };
  }
}

// Correct: Return object with next() method
class GoodIterable {
  [Symbol.iterator]() {
    let n = 0;
    return {
      next() {
        return { value: n++, done: n > 5 };
      }
    };
  }
}

// Wrong: Not handling done: true
const badIterator = {
  next() {
    return { value: 1 };  // Missing done property
  }
};

// Correct: Always return done property
const goodIterator = {
  next() {
    return { value: 1, done: false };
  }
};
```

## Quick Revision

- Iterator: object with `next()` returning `{ value, done }`
- Iterable: object with `[Symbol.iterator]()` returning an iterator
- Use generators (`function*`) for simpler iterator creation
- Infinite iterators are possible but must be consumed carefully
- Spread operator and `for...of` work with iterables
- Iterators are single-use (create new one for reuse)

## Related Topics

- [[What-is-Iterator]] - Iterator protocol
- [[Write-Generator]] - Generator functions
- [[Symbol-Iterator]] - Symbol.iterator
- [[Iterate-Map]] - Iterating Maps
