# What is Iterator Protocol

## Definition
The iterator protocol defines a standard way to produce a sequence of values. An object is an iterator if it has a `next()` method that returns `{ value: any, done: boolean }`.

## How Iterators Work

```javascript
const iterable = {
  [Symbol.iterator]() {
    let n = 0;
    return {
      next() {
        n++;
        if (n <= 5) {
          return { value: n, done: false };
        }
        return { value: undefined, done: true };
      }
    };
  }
};

// Using the iterator
for (const num of iterable) {
  console.log(num);  // 1, 2, 3, 4, 5
}
```

## The Iterator Protocol

An iterator must implement:

```javascript
// The iterator protocol requires:
const iterator = {
  // Returns { value: any, done: boolean }
  next() {
    return {
      value: "some value",
      done: false  // or true when complete
    };
  }
};
```

## Built-in Iterables

JavaScript has several built-in iterables:

```javascript
// Arrays
const arr = [1, 2, 3];
const arrIter = arr[Symbol.iterator]();
arrIter.next();  // { value: 1, done: false }

// Strings
const str = "hello";
const strIter = str[Symbol.iterator]();
strIter.next();  // { value: "h", done: false }

// Maps
const map = new Map([["a", 1]]);
const mapIter = map[Symbol.iterator]();
mapIter.next();  // { value: ["a", 1], done: false }

// Sets
const set = new Set([1, 2, 3]);
const setIter = set[Symbol.iterator]();
setIter.next();  // { value: 1, done: false }
```

## Creating Custom Iterables

```javascript
class Range {
  constructor(start, end) {
    this.start = start;
    this.end = end;
  }

  [Symbol.iterator]() {
    let current = this.start;
    const end = this.end;

    return {
      next() {
        if (current <= end) {
          return { value: current++, done: false };
        }
        return { value: undefined, done: true };
      }
    };
  }
}

// Using custom iterable
const range = new Range(1, 5);
for (const num of range) {
  console.log(num);  // 1, 2, 3, 4, 5
}
```

## Iterator Protocol vs Iterable Protocol

```javascript
// Iterable: Has Symbol.iterator method
// Iterator: Has next() method

class Fibonacci {
  constructor(limit) {
    this.limit = limit;
  }

  [Symbol.iterator]() {  // Makes it iterable
    let a = 0, b = 1, count = 0;
    const limit = this.limit;

    return {  // Returns an iterator
      next() {
        if (count >= limit) {
          return { value: undefined, done: true };
        }
        const value = a;
        [a, b] = [b, a + b];
        count++;
        return { value, done: false };
      }
    };
  }
}

const fib = new Fibonacci(8);
console.log([...fib]);  // [0, 1, 1, 2, 3, 5, 8, 13]
```

## Common Use Cases

### Lazy Evaluation

```javascript
function* numberGenerator(max) {
  let current = 0;
  while (current < max) {
    yield current++;
  }
}

// Only generates numbers as needed
const gen = numberGenerator(1000000);
console.log(gen.next().value);  // 0
console.log(gen.next().value);  // 1
```

### Custom Collection Iteration

```javascript
class TreeNode {
  constructor(value, left = null, right = null) {
    this.value = value;
    this.left = left;
    this.right = right;
  }

  *inOrder() {
    if (this.left) yield* this.left.inOrder();
    yield this.value;
    if (this.right) yield* this.right.inOrder();
  }
}
```

## Common Mistakes

```javascript
// Wrong: Forgetting to return done: true
const badIterator = {
  next() {
    return { value: 1 };  // Missing done property
  }
};

// Correct: Always include done property
const goodIterator = {
  next() {
    return { value: 1, done: false };
  }
};

// Wrong: Using next() directly in for-of
const iter = [1, 2, 3][Symbol.iterator]();
// Don't do this
for (const val of iter.next()) {}  // TypeError
```

## Quick Revision

- Iterator protocol: object with `next()` method
- `next()` returns `{ value, done }` object
- Iterable protocol: object with `Symbol.iterator` method
- Built-in iterables: Array, String, Map, Set, TypedArray
- Generators are easy ways to create iterators

## Related Topics

- [[Create-Iterator]] - Creating custom iterators
- [[Write-Generator]] - Generator function syntax
- [[Iterate-Map]] - Iterating Maps with iterators
- [[Symbol-Iterator]] - The Symbol.iterator well-known symbol
