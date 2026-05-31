# What is a Set?

## Definition

A Set is a **collection of unique values**.

## Creating Sets

```javascript
// Empty set
const set = new Set();

// With initial values
const set = new Set([1, 2, 3, 4, 5]);

// From string
const set = new Set("hello"); // Set(4) { "h", "e", "l", "o" }
```

## Set Methods

```javascript
const set = new Set();

// Add
set.add(1);
set.add(2);
set.add(3);

// Has
set.has(2); // true

// Delete
set.delete(2);

// Size
set.size; // 2

// Clear
set.clear();
```

## Iterating Sets

```javascript
const set = new Set([1, 2, 3]);

// forEach
set.forEach(value => {
    console.log(value);
});

// for...of
for (const value of set) {
    console.log(value);
}

// Convert to array
const arr = [...set];
```

## Common Use Cases

```javascript
// Remove duplicates
const arr = [1, 2, 2, 3, 3, 4];
const unique = [...new Set(arr)]; // [1, 2, 3, 4]

// Check existence
const set = new Set([1, 2, 3]);
set.has(2); // true (faster than array.includes)

// Set operations
const set1 = new Set([1, 2, 3]);
const set2 = new Set([2, 3, 4]);

// Intersection
const intersection = new Set([...set1].filter(x => set2.has(x)));

// Union
const union = new Set([...set1, ...set2]);
```

## Quick Revision

- Set = collection of unique values
- Methods: add, has, delete, size
- Automatically removes duplicates
- Faster than array for existence checks
- Use for: deduplication, fast lookups

---

## Related Topics

- [[What-is-Set]] - Set overview
- [[Use-Set]] - Using Set
- [[What-is-MapES6]] - Map
- [[What-is-Array]] - Arrays
- [[Remove-Duplicates]] - Removing duplicates
