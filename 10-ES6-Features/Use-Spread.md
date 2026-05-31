# Use Spread

## Definition

The spread operator (`...`) expands an iterable into individual elements. It's used to copy arrays, merge objects, and pass arguments to functions.

## Code Examples

### Spread Arrays

```javascript
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];

// Combine arrays
const combined = [...arr1, ...arr2];
console.log(combined); // [1, 2, 3, 4, 5, 6]

// Copy array
const copy = [...arr1];
console.log(copy); // [1, 2, 3]
```

### Spread Objects

```javascript
const obj1 = { a: 1, b: 2 };
const obj2 = { c: 3, d: 4 };

// Merge objects
const merged = { ...obj1, ...obj2 };
console.log(merged); // { a: 1, b: 2, c: 3, d: 4 }

// Override properties
const updated = { ...obj1, b: 99 };
console.log(updated); // { a: 1, b: 99 }
```

### Copy Arrays

```javascript
const original = [1, 2, 3];
const shallowCopy = [...original];

// Add elements
const withNew = [...original, 4, 5];
console.log(withNew); // [1, 2, 3, 4, 5]
```

### Function Arguments

```javascript
function sum(a, b, c) {
  return a + b + c;
}

const numbers = [1, 2, 3];
console.log(sum(...numbers)); // 6
```

### String to Array

```javascript
const str = 'hello';
const chars = [...str];
console.log(chars); // ['h', 'e', 'l', 'l', 'o']
```

### Merge with Override

```javascript
const defaults = { color: 'blue', size: 'medium' };
const userPrefs = { color: 'red' };

const settings = { ...defaults, ...userPrefs };
console.log(settings); // { color: 'red', size: 'medium' }
```

### Copy Nested Objects (Shallow)

```javascript
const original = {
  name: 'John',
  address: {
    city: 'New York'
  }
};

const copy = { ...original };
copy.name = 'Jane';
copy.address.city = 'Boston'; // Affects original!

console.log(original.address.city); // 'Boston'
```

## Common Use Cases

1. **Copying arrays/objects** - Create shallow copies
2. **Merging data** - Combine arrays or objects
3. **Passing arguments** - Expand array as function args
4. **Immutable updates** - Update state without mutation

## Common Mistakes

```javascript
// Wrong: Shallow copy of nested objects
const original = { a: 1, nested: { b: 2 } };
const copy = { ...original };
copy.nested.b = 99; // Modifies original!

// Correct: Deep copy
const deepCopy = {
  ...original,
  nested: { ...original.nested }
};

// Wrong: Overwriting properties in wrong order
const result = { ...obj2, ...obj1 }; // obj1 wins
```

## Related Topics

- [[Destructure-Objects]]
- [[Set-Defaults]]
- [[Use-LetConst]]

## Quick Revision

| Syntax | Purpose |
|--------|---------|
| `[...arr]` | Copy/expand array |
| `{...obj}` | Copy/merge object |
| `fn(...args)` | Spread function args |
| Last wins | When merging properties |
| Shallow copy | Doesn't deep clone nested objects |
