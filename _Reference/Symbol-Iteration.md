# Symbol.iterator

## Definition

Symbol.iterator defines the **default iteration behavior** for an object.

## Basic Usage

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
                return { done: true };
            }
        };
    }
}

const range = new Range(1, 5);
for (const num of range) {
    console.log(num); // 1, 2, 3, 4, 5
}
```

## How It Works

```javascript
// for...of uses Symbol.iterator
const arr = [1, 2, 3];
const iterator = arr[Symbol.iterator]();

iterator.next(); // { value: 1, done: false }
iterator.next(); // { value: 2, done: false }
iterator.next(); // { value: 3, done: false }
iterator.next(); // { value: undefined, done: true }
```

## Custom Iterables

```javascript
// String iterable
const str = "hello";
for (const char of str) {
    console.log(char); // h, e, l, l, o
}

// Map iterable
const map = new Map([["a", 1], ["b", 2]]);
for (const [key, value] of map) {
    console.log(key, value);
}

// Set iterable
const set = new Set([1, 2, 3]);
for (const num of set) {
    console.log(num);
}
```

## Quick Revision

- Symbol.iterator defines iteration behavior
- Returns object with next() method
- next() returns { value, done }
- Enables for...of loops
- Use for custom iterables

---

## Related Topics

- [[What-is-Iterator]] - [[What-is-Iterator|Iterator]] overview
- [[Create-Iterator]] - [[Create-Iterator|Creating iterators]]
- [[What-is-Symbol]] - [[What-is-Symbol|Symbol]]
- [[Write-Generator]] - [[Write-Generator|Generators]]
- [[What-is-ForOf]] - [[What-is-ForOf|For...of]]
