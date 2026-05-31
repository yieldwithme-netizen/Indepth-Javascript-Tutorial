# How to Write Generator Functions

## Definition
Generator functions are special functions that can be paused and resumed. They use `function*` syntax and the `yield` keyword to produce a sequence of values lazily.

## Basic Syntax

```javascript
function* numberGenerator() {
  yield 1;
  yield 2;
  yield 3;
}

const gen = numberGenerator();
console.log(gen.next());  // { value: 1, done: false }
console.log(gen.next());  // { value: 2, done: false }
console.log(gen.next());  // { value: 3, done: false }
console.log(gen.next());  // { value: undefined, done: true }
```

## How Generators Work

```javascript
function* simpleGenerator() {
  console.log("First execution");
  yield 1;

  console.log("After first yield");
  yield 2;

  console.log("After second yield");
  return 3;  // return sets value and done: true
}

const gen = simpleGenerator();
console.log("Generator created");

gen.next();  // "First execution" → { value: 1, done: false }
gen.next();  // "After first yield" → { value: 2, done: false }
gen.next();  // "After second yield" → { value: 3, done: true }
```

## Yield and Next

```javascript
function* twoWay() {
  const x = yield "First";
  const y = yield `Got: ${x}`;
  yield `Got: ${y}`;
}

const gen = twoWay();
console.log(gen.next());          // { value: "First", done: false }
console.log(gen.next("hello"));   // { value: "Got: hello", done: false }
console.log(gen.next("world"));   // { value: "Got: world", done: false }
console.log(gen.next());          // { value: undefined, done: true }
```

## Generator Expressions

```javascript
// Generator function expression
const gen = function* () {
  yield 1;
  yield 2;
  yield 3;
};

// Arrow functions CANNOT be generators
// const bad = *() => {};  // SyntaxError
```

## Common Use Cases

### Lazy Sequence Generation

```javascript
function* fibonacci() {
  let a = 0, b = 1;

  while (true) {
    yield a;
    [a, b] = [b, a + b];
  }
}

const fib = fibonacci();
console.log(fib.next().value);  // 0
console.log(fib.next().value);  // 1
console.log(fib.next().value);  // 1
console.log(fib.next().value);  // 2
console.log(fib.next().value);  // 3
```

### Async Iteration

```javascript
async function* fetchPages(urls) {
  for (const url of urls) {
    const response = await fetch(url);
    const data = await response.json();
    yield data;
  }
}

async function main() {
  const pages = fetchPages([
    "https://api.example.com/1",
    "https://api.example.com/2"
  ]);

  for await (const page of pages) {
    console.log(page);
  }
}
```

### State Machine

```javascript
function* trafficLight() {
  while (true) {
    yield "RED";
    yield "YELLOW";
    yield "GREEN";
  }
}

const light = trafficLight();
console.log(light.next().value);  // "RED"
console.log(light.next().value);  // "YELLOW"
console.log(light.next().value);  // "GREEN"
console.log(light.next().value);  // "RED" (loops)
```

### Iterating with yield*

```javascript
function* concat(iter1, iter2) {
  yield* iter1;
  yield* iter2;
}

function* range(start, end) {
  for (let i = start; i <= end; i++) {
    yield i;
  }
}

const combined = concat(range(1, 3), range(5, 7));
console.log([...combined]);  // [1, 2, 3, 5, 6, 7]
```

### Coroutine Pattern

```javascript
function* coroutine() {
  const x = yield 10;
  const y = yield 20;
  return x + y;
}

const gen = coroutine();
const a = gen.next().value;     // 10
const b = gen.next(a * 2).value; // 20 (sends 20 to x)
const result = gen.next(b * 3);  // { value: 200, done: true }
```

## Advanced Patterns

### Generator as Iterator

```javascript
class Range {
  constructor(start, end) {
    this.start = start;
    this.end = end;
  }

  *[Symbol.iterator]() {
    for (let i = this.start; i <= this.end; i++) {
      yield i;
    }
  }
}

const range = new Range(1, 5);
for (const num of range) {
  console.log(num);  // 1, 2, 3, 4, 5
}
```

### Error Handling in Generators

```javascript
function* safeGenerator() {
  try {
    yield 1;
    yield 2;
  } catch (e) {
    console.log("Error caught:", e.message);
  }
  yield 3;
}

const gen = safeGenerator();
console.log(gen.next());        // { value: 1, done: false }
console.log(gen.next());        // { value: 2, done: false }
console.log(gen.throw(new Error("Oops")));  // "Error caught: Oops" → { value: 3, done: false }
```

## Common Mistakes

```javascript
// Wrong: Using yield outside generator
function notAGenerator() {
  yield 1;  // SyntaxError: Unexpected identifier
}

// Wrong: Trying to use arrow function as generator
const gen = *() => {};  // SyntaxError

// Wrong: Not consuming generator
function* numbers() {
  yield 1;
  yield 2;
}

// This creates but doesn't run
numbers();  // Generator object, no values produced

// Correct: Must call .next() to execute
const gen = numbers();
gen.next();  // { value: 1, done: false }

// Wrong: Assuming generators are reusable
const gen2 = numbers();
[...gen2];  // [1, 2]
[...gen2];  // [] (already consumed)

// Correct: Create new generator for reuse
const gen3 = numbers();
[...gen3];  // [1, 2]
const gen4 = numbers();  // New generator
[...gen4];  // [1, 2]
```

## Quick Revision

- `function*` declares a generator function
- `yield` pauses execution and produces a value
- `yield*` delegates to another generator/iterable
- `.next()` resumes execution, returns `{ value, done }`
- `.throw()` sends errors into the generator
- `.return()` terminates the generator
- Generators are lazy (execute on demand)
- Used for iterators, async workflows, coroutines

## Related Topics

- [[What-is-Iterator]] - Iterator protocol
- [[Create-Iterator]] - Manual iterator creation
- [[Symbol-Iterator]] - Making objects iterable
- [[Async-Iteration]] - for-await-of loops
