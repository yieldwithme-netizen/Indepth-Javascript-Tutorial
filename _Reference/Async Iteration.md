# Async Iteration

## Definition

Async iteration **iterates over async iterables** using `for await...of`.

## Basic Syntax

```javascript
async function* generateNumbers() {
    yield 1;
    yield 2;
    yield 3;
}

async function main() {
    for await (const num of generateNumbers()) {
        console.log(num); // 1, 2, 3
    }
}
```

## With Async Generators

```javascript
async function* fetchPages(urls) {
    for (const url of urls) {
        const response = await fetch(url);
        yield await response.json();
    }
}

for await (const page of fetchPages(['/page1', '/page2'])) {
    console.log(page);
}
```

## Quick Revision

- `for await...of` for async iterables
- Works with async generators
- Use for: API calls, streams
- `yield` pauses, `await` waits

---

## Related Topics

- [[What-is-Async-Iteration]] - [[What-is-Async-Iteration|Async iteration]]
- [[Async Iteration]] - [[Async Iteration|Async iteration]]
- [[What-is-Generator]] - [[What-is-Generator|Generators]]
- [[Write-Generator]] - [[Write-Generator|Writing generators]]
