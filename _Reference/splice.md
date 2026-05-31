# Splice Method

## Definition

`splice()` **adds, removes, or replaces** elements at any position in an array.

## Syntax

```javascript
array.splice(start, deleteCount, item1, item2, ...)
```

- `start`: index to start
- `deleteCount`: number of elements to remove
- `item1, item2, ...`: elements to add

## Removing Elements

```javascript
const arr = [1, 2, 3, 4, 5];

// Remove 2 elements starting at index 1
const removed = arr.splice(1, 2);
console.log(removed); // [2, 3]
console.log(arr); // [1, 4, 5]

// Remove from index to end
const removed2 = arr.splice(1);
console.log(removed2); // [4, 5]
console.log(arr); // [1]
```

## Adding Elements

```javascript
const arr = [1, 2, 5];

// Insert at index 2 (no removal)
arr.splice(2, 0, 3, 4);
console.log(arr); // [1, 2, 3, 4, 5]
```

## Replacing Elements

```javascript
const arr = [1, 2, 3, 4, 5];

// Replace 2 elements at index 1
arr.splice(1, 2, "a", "b");
console.log(arr); // [1, "a", "b", 4, 5]
```

## Replacing All

```javascript
const arr = [1, 2, 3];

// Replace all elements
arr.splice(0, arr.length, "a", "b", "c");
console.log(arr); // ["a", "b", "c"]
```

## splice vs slice

| Feature | splice | slice |
|---------|--------|-------|
| Modifies original | Yes | No |
| Removes elements | Yes | No |
| Adds elements | Yes | No |
| Returns | Removed elements | Extracted portion |

## Quick Revision

- `splice(start, deleteCount, ...items)` modifies array
- Removes elements (returns removed)
- Adds elements at position
- Replaces elements
- Modifies original array (mutating)

---

## Related Topics

- [[What-is-Splice]] - [[What-is-Splice|Splice]] overview
- [[Use-Splice]] - [[Use-Splice|Using splice]]
- [[What-is-Slice]] - [[What-is-Slice|Slice]] (non-mutating)
- [[What-is-Array]] - [[What-is-Array|Arrays]]
- [[What-is-PushPop]] - [[What-is-PushPop|Push/pop]]
- [[What-is-ShiftUnshift]] - [[What-is-ShiftUnshift|Shift/unshift]]
