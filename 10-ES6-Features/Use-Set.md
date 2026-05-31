# How to Use Set Object

## Definition
A `Set` is a collection of unique values. It stores each value only once and maintains insertion order. Use Sets when you need to ensure no duplicates exist.

## Creating Sets

```javascript
// Empty set
const set1 = new Set();

// From array (removes duplicates)
const set2 = new Set([1, 2, 3, 2, 1]);
console.log(set2);  // Set { 1, 2, 3 }

// From string
const set3 = new Set("hello");
console.log(set3);  // Set { "h", "e", "l", "o" }
```

## Set Methods

### Add and Has

```javascript
const tags = new Set();

tags.add("javascript");
tags.add("tutorial");
tags.add("es6");
tags.add("javascript");  // Ignored (duplicate)

console.log(tags.size);  // 3

console.log(tags.has("javascript"));  // true
console.log(tags.has("react"));       // false
```

### Delete and Clear

```javascript
const set = new Set([1, 2, 3, 4, 5]);

set.delete(3);
console.log(set.has(3));  // false
console.log(set.size);    // 4

set.clear();
console.log(set.size);    // 0
```

## Iterating Sets

```javascript
const colors = new Set(["red", "green", "blue"]);

// for...of
for (const color of colors) {
  console.log(color);
}

// forEach
colors.forEach(color => {
  console.log(color);
});

// Spread to array
const colorsArray = [...colors];
console.log(colorsArray);  // ["red", "green", "blue"]
```

## Set Operations

### Union

```javascript
function union(setA, setB) {
  return new Set([...setA, ...setB]);
}

const setA = new Set([1, 2, 3]);
const setB = new Set([3, 4, 5]);

console.log(union(setA, setB));  // Set { 1, 2, 3, 4, 5 }
```

### Intersection

```javascript
function intersection(setA, setB) {
  return new Set([...setA].filter(x => setB.has(x)));
}

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([3, 4, 5, 6]);

console.log(intersection(setA, setB));  // Set { 3, 4 }
```

### Difference

```javascript
function difference(setA, setB) {
  return new Set([...setA].filter(x => !setB.has(x)));
}

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([3, 4, 5, 6]);

console.log(difference(setA, setB));  // Set { 1, 2 }
```

## Common Use Cases

### Removing Duplicates

```javascript
const numbers = [1, 2, 3, 2, 1, 4, 3, 5];
const unique = [...new Set(numbers)];
console.log(unique);  // [1, 2, 3, 4, 5]
```

### Track Visited Items

```javascript
function traverseGraph(start, graph) {
  const visited = new Set();
  const queue = [start];

  while (queue.length > 0) {
    const node = queue.shift();
    if (visited.has(node)) continue;

    visited.add(node);
    console.log(node);

    graph[node]?.forEach(neighbor => {
      if (!visited.has(neighbor)) {
        queue.push(neighbor);
      }
    });
  }
}
```

### Quick Membership Testing

```javascript
const validStatuses = new Set(["active", "inactive", "pending"]);

function isValidStatus(status) {
  return validStatuses.has(status);
}

console.log(isValidStatus("active"));   // true
console.log(isValidStatus("deleted"));  // false
```

### Set as Object Values

```javascript
class TagManager {
  constructor() {
    this.tags = new Map();
  }

  addTagToPost(postId, tag) {
    if (!this.tags.has(postId)) {
      this.tags.set(postId, new Set());
    }
    this.tags.get(postId).add(tag);
  }

  removeTagFromPost(postId, tag) {
    this.tags.get(postId)?.delete(tag);
  }

  getPostTags(postId) {
    return [...(this.tags.get(postId) || [])];
  }
}
```

## Common Mistakes

```javascript
// Wrong: Comparing objects in Set
const set = new Set();
const obj1 = { a: 1 };
const obj2 = { a: 1 };

set.add(obj1);
console.log(set.has(obj2));  // false (different reference)

// Correct: Keep reference
const key = { a: 1 };
set.add(key);
console.log(set.has(key));  // true

// Wrong: Using Set for key-value pairs
const badCache = new Set();
badCache.add("user:1");  // No way to store data with key

// Correct: Use Map for key-value
const goodCache = new Map();
goodCache.set("user:1", userData);
```

## Quick Revision

- `Set` stores unique values only
- `.add(value)` inserts, `.has(value)` checks existence
- `.delete(value)` removes, `.size` gives count
- Use `[...set]` to convert to array
- Great for deduplication and membership testing
- Use `Map` when you need key-value pairs

## Related Topics

- [[What-is-Set]] - Set object overview
- [[Use-Map]] - Map data structure
- [[Set-Operations]] - Union, intersection, difference
- [[WeakSet]] - WeakSet reference collection
