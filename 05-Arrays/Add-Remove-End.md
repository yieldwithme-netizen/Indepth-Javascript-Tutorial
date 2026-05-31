# How to Add/Remove from End

## push()

```javascript
const arr = [1, 2, 3];
arr.push(4);
console.log(arr); // [1, 2, 3, 4]

// Add multiple elements
arr.push(5, 6);
console.log(arr); // [1, 2, 3, 4, 5, 6]

// Returns new length
const length = arr.push(7);
console.log(length); // 7
```

## pop()

```javascript
const arr = [1, 2, 3, 4, 5];
const removed = arr.pop();
console.log(removed); // 5
console.log(arr); // [1, 2, 3, 4]

// Pop until empty
while (arr.length > 0) {
    console.log(arr.pop());
}
// Output: 4, 3, 2, 1
```

## Stack Implementation

```javascript
const stack = [];

// Push (add to top)
stack.push("A");
stack.push("B");
stack.push("C");
console.log(stack); // ["A", "B", "C"]

// Pop (remove from top)
stack.pop(); // "C"
stack.pop(); // "B"
console.log(stack); // ["A"]
```

## Performance

```javascript
// push/pop are O(1) - fast!
const arr = [];
for (let i = 0; i < 100000; i++) {
    arr.push(i); // Fast
}
```

## Quick Revision

- `push()` adds to end, returns new length
- `pop()` removes from end, returns removed element
- Both modify original array
- Use for stack (LIFO) implementation
- O(1) time complexity

---

## Related Topics

- [[What-is-PushPop]] - [[What-is-PushPop|Push/pop]] overview
- [[Add-Remove-End]] - [[Add-Remove-End|Adding/removing from end]]
- [[What-is-ShiftUnshift]] - [[What-is-ShiftUnshift|Shift/unshift]]
- [[Use-Splice]] - [[Use-Splice|Splice method]]
