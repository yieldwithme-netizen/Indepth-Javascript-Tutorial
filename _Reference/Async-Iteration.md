# Async Iteration

Async iteration allows you to iterate over asynchronous data sources like streams, promises, and async generators using `for await...of` loops.

## Definition

Async iteration extends JavaScript's iteration protocol to handle asynchronous data. It enables working with data that arrives over time without blocking the main thread.

## Async Iterators

```javascript
// Async iterator protocol: has next() returning Promise
const asyncIterator = {
    values: [1, 2, 3],
    index: 0,
    next() {
        if (this.index < this.values.length) {
            return Promise.resolve({
                value: this.values[this.index++],
                done: false
            });
        }
        return Promise.resolve({ done: true });
    }
};

// Using for await...of
async function iterate() {
    for await (const value of asyncIterator) {
        console.log(value); // 1, 2, 3
    }
}
```

## Async Generators

```javascript
// Async generator function
async function* asyncGenerator() {
    let i = 0;
    while (i < 3) {
        await new Promise(resolve => setTimeout(resolve, 1000));
        yield i++;
    }
}

// Using async generator
async function main() {
    for await (const value of asyncGenerator()) {
        console.log(value); // 0, 1, 2 (with 1s delay each)
    }
}

// Async generator with data fetching
async function* fetchPages(urls) {
    for (const url of urls) {
        const response = await fetch(url);
        const data = await response.json();
        yield data;
    }
}

// Usage
for await (const page of fetchPages(['/api/1', '/api/2', '/api/3'])) {
    console.log(page);
}
```

## Async Iterables

```javascript
// Object implementing Symbol.asyncIterator
const asyncIterable = {
    start: 0,
    end: 5,
    [Symbol.asyncIterator]() {
        let current = this.start;
        const end = this.end;
        
        return {
            async next() {
                if (current <= end) {
                    await new Promise(resolve => setTimeout(resolve, 500));
                    return { value: current++, done: false };
                }
                return { done: true };
            }
        };
    }
};

// Using
async function main() {
    for await (const num of asyncIterable) {
        console.log(num); // 0, 1, 2, 3, 4, 5 (with delay)
    }
}
```

## Common Use Cases

```javascript
// Processing streaming data
async function* readStream(stream) {
    const reader = stream.getReader();
    try {
        while (true) {
            const { done, value } = await reader.read();
            if (done) break;
            yield value;
        }
    } finally {
        reader.releaseLock();
    }
}

// Paginated API calls
async function* paginate(baseUrl) {
    let page = 1;
    let hasMore = true;
    
    while (hasMore) {
        const response = await fetch(`${baseUrl}?page=${page}`);
        const data = await response.json();
        yield data.items;
        hasMore = data.hasMore;
        page++;
    }
}

// Real-time updates
async function* socketMessages(socket) {
    while (true) {
        const message = await new Promise(resolve => {
            socket.once('message', resolve);
        });
        yield JSON.parse(message);
    }
}
```

## Error Handling

```javascript
async function* safeAsyncGenerator() {
    try {
        yield await asyncOperation();
    } catch (error) {
        console.error('Error in generator:', error);
        yield defaultValue;
    }
}

// Error handling in for await...of
async function main() {
    try {
        for await (const value of asyncGenerator()) {
            processValue(value);
        }
    } catch (error) {
        console.error('Iteration error:', error);
    }
}
```

## Common Use Cases

- Processing file streams
- Handling WebSocket messages
- Paginated API consumption
- Real-time data feeds
- Working with AsyncArrays

## Common Mistakes

1. **Using regular for...of with async iterables** - Must use `for await...of`
2. **Not handling errors** - Always wrap in try/catch
3. **Forgetting to close resources** - Use `finally` blocks for cleanup
4. **Blocking in async generators** - Avoid synchronous heavy operations
5. **Not returning Promises** - Async iterators must return Promises

## Related Topics

- [[Generators]]
- [[Promises]]
- [[Async/Await]]
- [[Streams]]
- [[Iteration Protocols]]

## Quick Revision

| Concept | Syntax |
|---------|--------|
| Async generator | `async function* gen() {}` |
| Async iteration | `for await (const x of asyncIter) {}` |
| Async iterator | `{ next() { return Promise<{value, done}> } }` |
| Symbol.asyncIterator | `[Symbol.asyncIterator]() {}` |
| Yield value | `yield value` (in async generator) |
