# How to Add/Remove from Start

## shift()

```javascript
const arr = [1, 2, 3, 4, 5];
const removed = arr.shift();
console.log(removed); // 1
console.log(arr); // [2, 3, 4, 5]
```

## unshift()

```javascript
const arr = [3, 4, 5];
arr.unshift(1, 2);
console.log(arr); // [1, 2, 3, 4, 5]

// Returns new length
const length = arr.unshift(0);
console.log(length); // 6
```

## Queue Implementation

```javascript
const queue = [];

// Enqueue (add to end)
queue.push("A");
queue.push("B");
queue.push("C");

// Dequeue (remove from front)
queue.shift(); // "A"
queue.shift(); // "B"
console.log(queue); // ["C"]
```

## Performance Warning

```javascript
// ❌ Slow: shift/unshift on large arrays
const arr = [];
for (let i = 0; i < 100000; i++) {
    arr.unshift(i); // O(n) - shifts all elements!
}

// ✅ Fast: push/pop on large arrays
const arr = [];
for (let i = 0; i < 100000; i++) {
    arr.push(i); // O(1) - adds to end
}
```

## Quick Revision

- `shift()` removes first element
- `unshift()` adds to beginning
- Both modify original array
- Use for queue (FIFO) implementation
- Slower than push/pop (shifts all elements)

---

## Related Topics

- [[What-is-ShiftUnshift]] - [[What-is-ShiftUnshift|Shift/unshift]] overview
- [[Add-Remove-Start]] - [[Add-Remove-Start|Adding/removing from start]]
- [[What-is-PushPop]] - [[What-is-PushPop|Push/pop]]
- [[Use-Splice]] - [[Use-Splice|Splice method]]
