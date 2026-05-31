# Generators

## Definition

Generators are **functions that pause execution** using `yield`.

## Basic Syntax

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
```

## Quick Revision

- Generator: `function*`
- Pauses with `yield`
- Resumes with `.next()`
- Returns `{value, done}`
- Use for: lazy evaluation

---

## Related Topics

- [[What-is-Generator]] - [[What-is-Generator|Generators]]
- [[Generators]] - [[Generators|Generators]]
- [[Write-Generator]] - [[Write-Generator|Writing generators]]
- [[What-is-Iterator]] - [[What-is-Iterator|Iterator]]
