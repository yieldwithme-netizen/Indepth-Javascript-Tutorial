# Iterators and Generators

## Definition

- **Iterator**: Object with `next()` method that returns `{value, done}`
- **Generator**: Function that pauses execution with `yield`

## Iterator Protocol

```javascript
const range = {
    start: 1,
    end: 5,
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
};

for (const num of range) {
    console.log(num); // 1, 2, 3, 4, 5
}
```

## Generator Functions

```javascript
function* count() {
    yield 1;
    yield 2;
    yield 3;
}

const gen = count();
gen.next(); // { value: 1, done: false }
gen.next(); // { value: 2, done: false }
gen.next(); // { value: 3, done: false }
gen.next(); // { value: undefined, done: true }
```

## Fibonacci Generator

```javascript
function* fibonacci() {
    let a = 0, b = 1;
    while (true) {
        yield a;
        [a, b] = [b, a + b];
    }
}

const fib = fibonacci();
console.log(fib.next().value); // 0
console.log(fib.next().value); // 1
console.log(fib.next().value); // 1
console.log(fib.next().value); // 2
```

## Quick Revision

- Iterator: `next()` returns `{value, done}`
- Generator: `function*` with `yield`
- Generators are iterators
- Use for: lazy evaluation, sequences
- `yield` pauses, `next()` resumes

---

## Related Topics

- [[What-is-Iterator]] - [[What-is-Iterator|Iterator]] overview
- [[Create-Iterator]] - [[Create-Iterator|Creating iterators]]
- [[What-is-Generator]] - [[What-is-Generator|Generators]]
- [[Write-Generator]] - [[Write-Generator|Writing generators]]
- [[Symbol-Iteration]] - [[Symbol-Iteration|Symbol.iterator]]
