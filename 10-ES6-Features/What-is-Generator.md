# What is a Generator Function?

## Definition

A generator function **pauses and resumes execution** using `yield`.

## Basic Syntax

```javascript
function* count() {
    yield 1;
    yield 2;
    yield 3;
}

const gen = count();
console.log(gen.next()); // { value: 1, done: false }
console.log(gen.next()); // { value: 2, done: false }
console.log(gen.next()); // { value: 3, done: false }
console.log(gen.next()); // { value: undefined, done: true }
```

## How It Works

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
console.log(fib.next().value); // 3
```

## Iterating Generators

```javascript
function* numbers() {
    yield 1;
    yield 2;
    yield 3;
}

// for...of
for (const num of numbers()) {
    console.log(num); // 1, 2, 3
}

// Spread
const arr = [...numbers()]; // [1, 2, 3]
```

## Quick Revision

- Generator: `function*` syntax
- Pauses with `yield`
- Resumes with `.next()`
- Returns `{ value, done }`
- Use for: lazy evaluation, iterators

---

## Related Topics

- [[What-is-Generator]] - Generator overview
- [[Write-Generator]] - Writing generators
- [[What-is-Iterator]] - Iterator protocol
- [[What-is-Function]] - Functions
- [[What-is-AsyncAwait]] - Async/await
