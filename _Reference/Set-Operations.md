# Set Operations in JavaScript

## Definition

**Sets** are collections of unique values. JavaScript's `Set` object provides efficient operations for working with unique data, including union, intersection, difference, and symmetric difference. Sets are useful for deduplication, membership testing, and mathematical set operations.

---

## Creating Sets

```javascript
// Empty Set
const set1 = new Set();

// From array (removes duplicates)
const set2 = new Set([1, 2, 3, 3, 4, 4, 5]);
console.log(set2); // Set {1, 2, 3, 4, 5}

// From string
const set3 = new Set("hello");
console.log(set3); // Set {"h", "e", "l", "o"}

// From arguments
function createSet() {
  return new Set(arguments);
}
createSet(1, 2, 3); // Set {1, 2, 3}
```

---

## Basic Set Methods

```javascript
const set = new Set();

// Add elements
set.add(1);
set.add(2);
set.add(3);
set.add(2); // Ignored (already exists)
console.log(set); // Set {1, 2, 3}

// Check existence
console.log(set.has(2)); // true
console.log(set.has(4)); // false

// Delete elements
set.delete(2);
console.log(set); // Set {1, 3}

// Size
console.log(set.size); // 2

// Clear all
set.clear();
console.log(set.size); // 0
```

---

## Iterating Sets

```javascript
const fruits = new Set(["apple", "banana", "cherry"]);

// forEach
fruits.forEach(fruit => {
  console.log(fruit);
});

// for...of
for (const fruit of fruits) {
  console.log(fruit);
}

// Convert to array
const fruitsArray = Array.from(fruits);
const fruitsArray2 = [...fruits];

// Keys and values (same for Set)
console.log([...fruits.keys()]); // ["apple", "banana", "cherry"]
console.log([...fruits.values()]); // ["apple", "banana", "cherry"]
console.log([...fruits.entries()]); // [["apple", "apple"], ...]
```

---

## Set Operations

### Union (A ∪ B)

```javascript
function union(setA, setB) {
  return new Set([...setA, ...setB]);
}

const a = new Set([1, 2, 3]);
const b = new Set([3, 4, 5]);
console.log(union(a, b)); // Set {1, 2, 3, 4, 5}
```

### Intersection (A ∩ B)

```javascript
function intersection(setA, setB) {
  return new Set([...setA].filter(x => setB.has(x)));
}

const a = new Set([1, 2, 3, 4]);
const b = new Set([3, 4, 5, 6]);
console.log(intersection(a, b)); // Set {3, 4}
```

### Difference (A - B)

```javascript
function difference(setA, setB) {
  return new Set([...setA].filter(x => !setB.has(x)));
}

const a = new Set([1, 2, 3, 4]);
const b = new Set([3, 4, 5, 6]);
console.log(difference(a, b)); // Set {1, 2}
```

### Symmetric Difference (A △ B)

```javascript
function symmetricDifference(setA, setB) {
  return new Set([
    ...[...setA].filter(x => !setB.has(x)),
    ...[...setB].filter(x => !setA.has(x))
  ]);
}

const a = new Set([1, 2, 3, 4]);
const b = new Set([3, 4, 5, 6]);
console.log(symmetricDifference(a, b)); // Set {1, 2, 5, 6}
```

### Subset and Superset

```javascript
function isSubset(setA, setB) {
  return [...setA].every(x => setB.has(x));
}

function isSuperset(setA, setB) {
  return [...setB].every(x => setA.has(x));
}

const a = new Set([1, 2]);
const b = new Set([1, 2, 3, 4]);
console.log(isSubset(a, b)); // true
console.log(isSuperset(b, a)); // true
```

---

## Common Use Cases

### Deduplication

```javascript
// Remove duplicates from array
const numbers = [1, 2, 2, 3, 4, 4, 5];
const unique = [...new Set(numbers)]; // [1, 2, 3, 4, 5]

// Remove duplicates from string
const str = "hello world";
const uniqueChars = [...new Set(str)].join(""); // "helo wrd"

// Remove duplicates from object array
const users = [
  { id: 1, name: "Alice" },
  { id: 2, name: "Bob" },
  { id: 1, name: "Alice" }
];
const uniqueUsers = [...new Map(users.map(u => [u.id, u])).values()];
```

### Fast Membership Testing

```javascript
// Instead of array.includes() (O(n))
const allowed = new Set(["admin", "editor", "user"]);

function hasPermission(role) {
  return allowed.has(role); // O(1)
}

// Example with large dataset
const validIds = new Set([1, 2, 3, 4, 5]);
const idsToCheck = [1, 2, 6, 7, 3];

const valid = idsToCheck.filter(id => validIds.has(id)); // [1, 2, 3]
```

### Data Processing

```javascript
// Find common elements
const array1 = [1, 2, 3, 4, 5];
const array2 = [4, 5, 6, 7, 8];
const common = [...new Set(array1).intersection(new Set(array2))]; // [4, 5]

// Group by category
const items = [
  { id: 1, category: "fruit" },
  { id: 2, category: "vegetable" },
  { id: 3, category: "fruit" }
];

const categories = new Set(items.map(i => i.category));
// Set {"fruit", "vegetable"}

// Check for duplicates
function hasDuplicates(arr) {
  return arr.length !== new Set(arr).size;
}

hasDuplicates([1, 2, 3]); // false
hasDuplicates([1, 2, 2]); // true
```

### State Management

```javascript
// Track selected items
const selected = new Set();

function toggleItem(id) {
  if (selected.has(id)) {
    selected.delete(id);
  } else {
    selected.add(id);
  }
  return new Set(selected); // Return copy for immutability
}

function selectAll(ids) {
  return new Set(ids);
}

function clearSelection() {
  return new Set();
}
```

---

## WeakSet

```javascript
// WeakSet only stores objects (allow garbage collection)
const weakSet = new WeakSet();

const obj1 = { name: "Alice" };
const obj2 = { name: "Bob" };

weakSet.add(obj1);
weakSet.add(obj2);

console.log(weakSet.has(obj1)); // true

// Can't iterate WeakSet
// for (const item of weakSet) {} // TypeError

// Useful for private data
const privateData = new WeakSet();

class User {
  constructor(name) {
    this.name = name;
    privateData.add(this);
  }
  
  getSecret() {
    if (privateData.has(this)) {
      return "secret";
    }
    return null;
  }
}
```

---

## Common Mistakes

### Mistake 1: Comparing Objects

```javascript
const set = new Set();
const obj1 = { id: 1 };
const obj2 = { id: 1 };

set.add(obj1);
console.log(set.has(obj1)); // true
console.log(set.has(obj2)); // false (different reference)

// Solution: use primitive values or Map
const map = new Map();
map.set(obj1.id, obj1);
console.log(map.has(1)); // true
```

### Mistake 2: Set Equality

```javascript
const set1 = new Set([1, 2, 3]);
const set2 = new Set([1, 2, 3]);

console.log(set1 === set2); // false (different references)

// Solution: compare manually
function setsEqual(a, b) {
  if (a.size !== b.size) return false;
  return [...a].every(x => b.has(x));
}

setsEqual(set1, set2); // true
```

### Mistake 3: Modifying During Iteration

```javascript
const set = new Set([1, 2, 3, 4, 5]);

// Wrong: modifying during iteration
for (const item of set) {
  if (item % 2 === 0) {
    set.delete(item); // May skip items
  }
}

// Correct: collect items first
const toDelete = [...set].filter(x => x % 2 === 0);
toDelete.forEach(x => set.delete(x));
```

### Mistake 4: Using Set for Ordered Data

```javascript
// Set maintains insertion order, but...
const set = new Set([3, 1, 2]);
console.log([...set]); // [3, 1, 2] (insertion order)

// Don't rely on Set for sorting
const sorted = [...set].sort((a, b) => a - b); // [1, 2, 3]
```

---

## Quick Revision Summary

| Method | Description |
|--------|-------------|
| `add(value)` | Add element |
| `has(value)` | Check existence |
| `delete(value)` | Remove element |
| `clear()` | Remove all |
| `size` | Number of elements |
| `forEach()` | Iterate |
| `[...set]` | Convert to array |

---

## Related Topics

- [[WeakSet]] - Weak references in Sets
- [[Array-Access]] - Array methods
- [[Object]] - Object as key-value store
- [[Map]] - Key-value pairs
- [[expression]] - Set expressions
- [[loop]] - Iterating Sets
- [[Destructure-Arrays]] - Destructuring Sets