# Array Destructuring

## Definition

Array destructuring is an ES6 feature that allows you to unpack values from arrays into distinct variables. It provides a concise syntax to extract multiple array elements in a single statement, replacing verbose indexing operations.

## Basic Syntax

```javascript
// Without destructuring
const numbers = [1, 2, 3];
const a = numbers[0];
const b = numbers[1];
const c = numbers[2];

// With destructuring
const [x, y, z] = [1, 2, 3];
console.log(x, y, z); // 1 2 3
```

## Examples

### 1. Basic Array Destructuring

```javascript
const colors = ['red', 'green', 'blue'];
const [first, second, third] = colors;

console.log(first);  // "red"
console.log(second); // "green"
console.log(third);  // "blue"
```

### 2. Skipping Elements

```javascript
const numbers = [1, 2, 3, 4, 5];

// Skip the second element
const [a, , c, , e] = numbers;
console.log(a, c, e); // 1 3 5

// Skip first two
const [, , third] = numbers;
console.log(third); // 3
```

### 3. Default Values

```javascript
const [a = 10, b = 20, c = 30] = [1, 2];

console.log(a); // 1
console.log(b); // 2
console.log(c); // 30 (uses default)

// Useful with undefined values
const [x = 'default'] = [undefined];
console.log(x); // "default"
```

### 4. Rest Pattern

```javascript
const [first, second, ...rest] = [1, 2, 3, 4, 5];

console.log(first);  // 1
console.log(second); // 2
console.log(rest);   // [3, 4, 5]

// Sum remaining elements
const [head, ...tail] = [10, 20, 30, 40];
const sum = tail.reduce((acc, val) => acc + val, 0);
console.log(sum); // 90
```

### 5. Swapping Variables

```javascript
let a = 1;
let b = 2;

// Without temporary variable
[a, b] = [b, a];

console.log(a, b); // 2 1

// Swap more variables
let x = 1, y = 2, z = 3;
[x, y, z] = [z, x, y];
console.log(x, y, z); // 3 1 2
```

### 6. Nested Array Destructuring

```javascript
const matrix = [
  [1, 2],
  [3, 4],
  [5, 6]
];

const [[a, b], [c, d], [e, f]] = matrix;
console.log(a, b, c, d, e, f); // 1 2 3 4 5 6

// Deep nesting
const nested = [1, [2, [3, 4]], 5];
const [first, [second, [third, fourth]], fifth] = nested;
console.log(first, second, third, fourth, fifth);
// 1 2 3 4 5
```

### 7. Function Return Values

```javascript
function getCoordinates() {
  return [10, 20, 30];
}

const [x, y, z] = getCoordinates();
console.log(x, y, z); // 10 20 30

// With default values
function getUser() {
  return ['Alice', 25, 'alice@email.com'];
}

const [name, age = 18, email = 'N/A', phone = 'N/A'] = getUser();
console.log(name, age, email, phone);
// "Alice" 25 "alice@email.com" "N/A"
```

### 8. Iterating with Destructuring

```javascript
const fruits = ['apple', 'banana', 'cherry', 'date'];

for (const [index, fruit] of fruits.entries()) {
  console.log(`${index}: ${fruit}`);
}
// 0: apple
// 1: banana
// 2: cherry
// 3: date
```

### 9. Parsing Array Results

```javascript
// Split string
const fullName = 'John Doe';
const [firstName, lastName] = fullName.split(' ');
console.log(firstName, lastName); // "John" "Doe"

// RegExp matches
const date = '2024-01-15';
const [year, month, day] = date.split('-');
console.log(year, month, day); // "2024" "01" "15"
```

### 10. Combined with Spread

```javascript
const original = [1, 2, 3];
const [first, ...remaining] = original;
const copy = [...remaining];

console.log(first);    // 1
console.log(remaining); // [2, 3]
console.log(copy);      // [2, 3]
```

## Common Use Cases

### Extracting API Response Data

```javascript
// API returns array of results
const response = [
  { id: 1, name: 'Product 1' },
  { id: 2, name: 'Product 2' },
  { id: 3, name: 'Product 3' }
];

const [firstProduct, secondProduct] = response;
console.log(firstProduct.name); // "Product 1"
```

### State Management

```javascript
// React-like state with useState
const [count, setCount] = useState(0);

// Custom hook
function useToggle(initial = false) {
  const [value, setValue] = useState(initial);
  const toggle = () => setValue(v => !v);
  return [value, toggle];
}

const [isOpen, setIsOpen] = useToggle(false);
```

### Processing Collections

```javascript
const scores = [85, 92, 78, 95, 88];

// Get min and max
const [min, ...rest] = scores.sort((a, b) => a - b);
const max = rest[rest.length - 1];

// Or simpler
const sorted = [...scores].sort((a, b) => a - b);
const [lowest, ...others] = sorted;
const highest = others[others.length - 1];
```

## Common Mistakes

### 1. Not Providing Enough Elements

```javascript
// WRONG: May get undefined
const [a, b, c] = [1, 2];
console.log(c); // undefined

// RIGHT: Use default values
const [a = 0, b = 0, c = 0] = [1, 2];
console.log(c); // 0
```

### 2. Wrong Order of Elements

```javascript
// WRONG: Order matters
const [firstName, lastName] = ['Doe', 'John'];
console.log(firstName); // "Doe" - Wrong!

// RIGHT: Match the order
const [firstName, lastName] = ['John', 'Doe'];
console.log(firstName); // "John"
```

### 3. Trying to Destructure Non-Arrays

```javascript
// WRONG: String is not an array
const [a, b, c] = 'hello';
// a = 'h', b = 'e', c = 'l' - Works but unexpected

// RIGHT: Convert to array first
const chars = [...'hello'];
const [first, second, ...rest] = chars;
```

### 4. Forgetting Rest Operator Creates New Array

```javascript
const numbers = [1, 2, 3, 4, 5];
const [first, ...rest] = numbers;

rest.push(6);
console.log(numbers); // [1, 2, 3, 4, 5] - unchanged
console.log(rest);    // [2, 3, 4, 5, 6]
```

## Quick Revision Summary

- Extract array elements into variables: `const [a, b] = [1, 2]`
- Skip elements: `const [a, , c] = [1, 2, 3]`
- Default values: `const [a = 10] = []`
- Rest pattern: `const [a, ...rest] = [1, 2, 3]`
- Swap variables: `[a, b] = [b, a]`
- Works with functions returning arrays
- Preserves original array (non-destructive)

## Related Topics

- [[Object-Destructuring]] - Destructuring objects
- [[Spread-Operator]] - Spread syntax for arrays
- [[Rest-Parameters]] - Function rest parameters
- [[ES6-Features]] - Modern JavaScript features
- [[Array-Methods]] - Built-in array methods
- [[Template-Literals]] - String interpolation
