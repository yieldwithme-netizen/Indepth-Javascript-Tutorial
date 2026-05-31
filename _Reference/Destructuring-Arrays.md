# Destructuring Arrays

Array destructuring is an ES6 feature that allows you to extract values from arrays and assign them to variables in a concise syntax.

## Basic Syntax

```javascript
// Old way
const numbers = [1, 2, 3];
const a = numbers[0];
const b = numbers[1];
const c = numbers[2];

// Destructuring
const [x, y, z] = [1, 2, 3];
console.log(x); // 1
console.log(y); // 2
console.log(z); // 3
```

## Skipping Elements

```javascript
const colors = ['red', 'green', 'blue', 'yellow'];

const [first, , third] = colors;
console.log(first); // 'red'
console.log(third); // 'blue'

// Skip multiple elements
const [, , , last] = colors;
console.log(last); // 'yellow'
```

## Rest Pattern

```javascript
const numbers = [1, 2, 3, 4, 5];

const [head, ...tail] = numbers;
console.log(head); // 1
console.log(tail); // [2, 3, 4, 5]

// First two, rest as array
const [first, second, ...rest] = numbers;
console.log(first);  // 1
console.log(second); // 2
console.log(rest);   // [3, 4, 5]
```

## Default Values

```javascript
const [a = 10, b = 20, c = 30] = [1, 2];
console.log(a); // 1
console.log(b); // 2
console.log(c); // 30 (uses default)

// With undefined values
const [x = 'default'] = [undefined];
console.log(x); // 'default'

// With null values (null is not undefined)
const [y = 'default'] = [null];
console.log(y); // null
```

## Swapping Variables

```javascript
let a = 1;
let b = 2;

// Traditional way
let temp = a;
a = b;
b = temp;

// With destructuring
[a, b] = [b, a];
console.log(a); // 2
console.log(b); // 1
```

## Nested Arrays

```javascript
const matrix = [
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9]
];

const [[, second], [fourth]] = matrix;
console.log(second); // 2
console.log(fourth); // 4

// Deep nesting
const [a, [b, [c, d]]] = [1, [2, [3, 4]]];
console.log(c); // 3
console.log(d); // 4
```

## Function Return Values

```javascript
function getCoordinates() {
  return [40.7128, -74.0060];
}

const [latitude, longitude] = getCoordinates();
console.log(latitude);  // 40.7128
console.log(longitude); // -74.0060

// Multiple return values
function useState(initial) {
  let value = initial;
  const getValue = () => value;
  const setValue = (newValue) => { value = newValue; };
  return [getValue, setValue];
}

const [getName, setName] = useState('Alice');
console.log(getName()); // 'Alice'
setName('Bob');
console.log(getName()); // 'Bob'
```

## Iterating with Destructuring

```javascript
const fruits = ['apple', 'banana', 'cherry'];

// for...of with destructuring
for (const [index, fruit] of fruits.entries()) {
  console.log(`${index}: ${fruit}`);
}

// Map entries
const person = { name: 'Alice', age: 25 };
for (const [key, value] of Object.entries(person)) {
  console.log(`${key}: ${value}`);
}
```

## Practical Examples

### Parsing CSV Data

```javascript
function parseCSV(csv) {
  return csv.split('\n').map(row => {
    const [name, age, email] = row.split(',');
    return { name, age: parseInt(age), email };
  });
}

const data = parseCSV('Alice,25,alice@email.com\nBob,30,bob@email.com');
```

### Array Chunking

```javascript
function chunk(array, size) {
  const chunks = [];
  for (let i = 0; i < array.length; i += size) {
    chunks.push(array.slice(i, i + size));
  }
  return chunks;
}

const [[a, b], [c, d]] = chunk([1, 2, 3, 4, 5, 6], 2);
```

### Array Flattening

```javascript
function flatten(arrays) {
  return arrays.reduce((acc, arr) => [...acc, ...arr], []);
}

const [first, ...rest] = flatten([[1, 2], [3, 4], [5]]);
```

## Common Use Cases

- Extracting function return values
- Parsing API responses
- Swapping variables
- Working with arrays of arrays
- State management (React hooks)

## Common Mistakes

1. **Too many variables** - Only destructure what you need
2. **Ignoring defaults** - Set defaults for potentially missing values
3. **Over-nesting** - Keep destructuring readable
4. **Performance** - Don't destructure in tight loops
5. **Mutating** - Destructuring doesn't modify original array

## Related Topics

- [[Destructuring]]
- [[Spread-Operator]]
- [[Rest-Parameters]]
- [[Array-Methods]]
- [[ES6-Features]]

## Quick Revision

| Pattern | Example | Result |
|---------|---------|--------|
| Basic | `[a, b] = [1, 2]` | a=1, b=2 |
| Skip | `[a, , c] = [1,2,3]` | a=1, c=3 |
| Rest | `[a, ...b] = [1,2,3]` | a=1, b=[2,3] |
| Default | `[a=10] = []` | a=10 |
| Swap | `[a,b] = [b,a]` | Swapped |

Array destructuring makes code more readable and concise when working with arrays.