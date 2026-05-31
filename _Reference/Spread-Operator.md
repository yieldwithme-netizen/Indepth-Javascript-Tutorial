# Spread Operator

## Definition

The spread operator (`...`) expands an iterable (array, string, object) into individual elements or properties. Introduced in ES6, it provides a concise syntax for copying, merging, and spreading data structures. The spread operator is similar to the rest parameter but works in the opposite direction.

## Code Examples

### Spread in Array Literals

```javascript
// Copying an array
const original = [1, 2, 3];
const copy = [...original];
console.log(copy); // [1, 2, 3]

// Merging arrays
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const merged = [...arr1, ...arr2];
console.log(merged); // [1, 2, 3, 4, 5, 6]

// Adding elements
const numbers = [2, 3, 4];
const withOne = [1, ...numbers];
console.log(withOne); // [1, 2, 3, 4]
```

### Spread in Function Calls

```javascript
// Passing array as arguments
function sum(a, b, c) {
  return a + b + c;
}

const nums = [1, 2, 3];
console.log(sum(...nums)); // 6

// Math functions
const temperatures = [72, 68, 75, 80, 65];
console.log(Math.max(...temperatures)); // 80
console.log(Math.min(...temperatures)); // 65
```

### Spread in Object Literals

```javascript
// Copying an object
const original = { a: 1, b: 2, c: 3 };
const copy = { ...original };
console.log(copy); // { a: 1, b: 2, c: 3 }

// Merging objects (later properties override earlier)
const defaults = { color: "red", size: 10, theme: "light" };
const userPrefs = { color: "blue", theme: "dark" };
const config = { ...defaults, ...userPrefs };
console.log(config); // { color: "blue", size: 10, theme: "dark" }

// Adding properties
const person = { name: "Alice", age: 30 };
const withCity = { ...person, city: "NYC" };
console.log(withCity); // { name: "Alice", age: 30, city: "NYC" }

// Overriding properties
const updated = { ...person, age: 31 };
console.log(updated); // { name: "Alice", age: 31 }
```

### Spread with Strings

```javascript
// Spreading a string into characters
const chars = [..."hello"];
console.log(chars); // ["h", "e", "l", "l", "o"]

// Convert to array
const word = "world";
const wordArray = Array.from(word);
console.log(wordArray); // ["w", "o", "r", "l", "d"]
```

### Spread vs Rest

```javascript
// Rest parameter - collects arguments
function collect(...args) {
  return args;
}
collect(1, 2, 3); // [1, 2, 3]

// Spread operator - expands an array
const arr = [1, 2, 3];
function display(a, b, c) {
  console.log(a, b, c);
}
display(...arr); // 1 2 3
```

### Shallow Copy Implications

```javascript
// Spread creates a SHALLOW copy
const original = {
  name: "Alice",
  address: { city: "NYC", state: "NY" },
};

const copy = { ...original };
copy.name = "Bob"; // Doesn't affect original
copy.address.city = "LA"; // Affects original!

console.log(original.address.city); // "LA" - mutated!
```

### Common Patterns

```javascript
// Remove first element
const [first, ...rest] = [1, 2, 3, 4];
console.log(first); // 1
console.log(rest);  // [2, 3, 4]

// Clone array without reference
const arr = [1, 2, 3];
const clone = [...arr];
clone.push(4);
console.log(arr); // [1, 2, 3] - original unchanged

// Merge with conditional
const includeB = true;
const result = ["a", ...(includeB ? ["b"] : []), "c"];
console.log(result); // ["a", "b", "c"]

// Convert arguments to array
function example() {
  const args = [...arguments];
  return args;
}
```

## Common Use Cases

- **Array copying** — Create shallow copies of arrays
- **Array merging** — Combine multiple arrays
- **Object copying** — Clone objects
- **Object merging** — Combine configurations
- **Function arguments** — Pass arrays as function parameters
- **Removing array elements** — Extract and skip elements
- **DOM manipulation** — Convert NodeLists to arrays

## Common Mistakes

```javascript
// Mistake 1: Deep copy assumption
const original = { nested: { value: 1 } };
const copy = { ...original };
copy.nested.value = 2;
console.log(original.nested.value); // 2 - mutated!

// Fix: use structuredClone() or manual deep copy
const deepCopy = structuredClone(original);

// Mistake 2: Spread in wrong context
// const arr = ...[1, 2, 3]; // SyntaxError
// const arr = [...[1, 2, 3]]; // Correct

// Mistake 3: Performance with large data
const hugeArray = new Array(1000000).fill(0);
// const copy = [...hugeArray]; // Slow, use slice() instead
const fastCopy = hugeArray.slice(); // Faster

// Mistake 4: Losing methods when spreading
class MyClass {
  method() {
    return "hello";
  }
}
const instance = new MyClass();
const spread = { ...instance };
console.log(typeof spread.method); // "undefined" - methods lost!
```

## Related Topics

- [[Rest-Parameters]]
- [[Destructuring]]
- [[Array-Methods]]
- [[Object-Methods]]
- [[ES6-Features]]
- [[Arrow-Functions]]
- [[Define-Objects]]

## Quick Revision

| Context | Syntax | Effect |
|---------|--------|--------|
| Arrays | `[...arr]` | Expands into elements |
| Objects | `{...obj}` | Expands into properties |
| Functions | `fn(...args)` | Expands into arguments |
| Strings | `[...str]` | Splits into characters |

| Feature | Description |
|---------|-------------|
| Shallow copy | Creates new reference, shares nested objects |
| Merge order | Later values override earlier |
| Works with | Arrays, Objects, Strings, Iterables |
| ES6 | Introduced in ECMAScript 2015 |
| Rest | Opposite direction — collects into array |
