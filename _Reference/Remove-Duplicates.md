# Remove Duplicates

## Definition

Removing duplicates creates a **unique set** of values from an array.

## Using Set

```javascript
const arr = [1, 2, 2, 3, 3, 4, 5, 5];
const unique = [...new Set(arr)];
console.log(unique); // [1, 2, 3, 4, 5]
```

## Using filter

```javascript
const arr = [1, 2, 2, 3, 3, 4, 5, 5];
const unique = arr.filter((item, index) => arr.indexOf(item) === index);
console.log(unique); // [1, 2, 3, 4, 5]
```

## Using reduce

```javascript
const arr = [1, 2, 2, 3, 3, 4, 5, 5];
const unique = arr.reduce((acc, item) => {
    if (!acc.includes(item)) {
        acc.push(item);
    }
    return acc;
}, []);
console.log(unique); // [1, 2, 3, 4, 5]
```

## Objects in Array

```javascript
const arr = [
    { id: 1, name: "John" },
    { id: 2, name: "Jane" },
    { id: 1, name: "John" }
];

const unique = arr.filter((item, index, self) =>
    index === self.findIndex(t => t.id === item.id)
);
console.log(unique); // [{id: 1, name: "John"}, {id: 2, name: "Jane"}]
```

## Quick Revision

- `[...new Set(arr)]` - fastest method
- `filter()` - preserves original
- `reduce()` - functional approach
- Objects: use `findIndex()`
- Time complexity: Set = O(n), filter = O(n²)

---

## Related Topics

- [[What-is-Array]] - [[What-is-Array|Arrays]]
- [[What-is-Set]] - [[What-is-Set|Set]]
- [[Use-Set]] - [[Use-Set|Using Set]]
- [[Use-Filter]] - [[Use-Filter|Filter]]
- [[Set-Operations]] - [[Set-Operations|Set operations]]
