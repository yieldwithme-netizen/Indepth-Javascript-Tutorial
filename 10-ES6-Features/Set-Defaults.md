# Set Defaults

## Definition

Default parameters allow you to set fallback values for function parameters when no value or `undefined` is passed. This simplifies function signatures and eliminates the need for manual parameter checking.

## Code Examples

### Basic Defaults

```javascript
function greet(name = 'Guest') {
  console.log(`Hello, ${name}!`);
}

greet('John'); // Hello, John!
greet();      // Hello, Guest!
```

### Multiple Defaults

```javascript
function createUser(name = 'Anonymous', age = 0, role = 'user') {
  return { name, age, role };
}

createUser('John', 25, 'admin'); // { name: 'John', age: 25, role: 'admin' }
createUser('Jane', 30);          // { name: 'Jane', age: 30, role: 'user' }
createUser();                    // { name: 'Anonymous', age: 0, role: 'user' }
```

### Defaults with Destructuring

```javascript
function greet({ name = 'Guest', age = 0 } = {}) {
  console.log(`Hello ${name}, age ${age}`);
}

greet({ name: 'John', age: 30 }); // Hello John, age 30
greet({ name: 'Jane' });          // Hello Jane, age 0
greet();                          // Hello Guest, age 0
```

### Defaults with Spread

```javascript
function mergeObjects(obj1 = {}, obj2 = {}) {
  return { ...obj1, ...obj2 };
}

mergeObjects({ a: 1 }, { b: 2 }); // { a: 1, b: 2 }
mergeObjects({ a: 1 });           // { a: 1 }
mergeObjects();                    // {}
```

### Computed Defaults

```javascript
function getDefaultId() {
  return Math.random().toString(36).substr(2, 9);
}

function createItem(name, id = getDefaultId()) {
  return { name, id };
}

createItem('Item 1'); // { name: 'Item 1', id: 'k7x2m9p1q' }
```

### Arrow Functions with Defaults

```javascript
const multiply = (a, b = 1) => a * b;

multiply(5, 3); // 15
multiply(5);    // 5
```

### Handling Undefined vs Null

```javascript
function test(value = 'default') {
  console.log(value);
}

test(undefined); // 'default'
test(null);      // null (not replaced by default)
test();          // 'default'
```

## Common Use Cases

1. **Configuration objects** - Default options
2. **Optional arguments** - Fallback values
3. **API parameters** - Default query options
4. **Component props** - Default React props

## Common Mistakes

```javascript
// Wrong: Using default with object parameter
function process(data = {}) {
  const { a, b } = data;
}

// Better: Destructure with defaults
function process({ a = 1, b = 2 } = {}) {
  console.log(a, b);
}

// Wrong: Not handling null
function test(value = 'default') {
  return value; // null stays null
}

// Correct: Explicit null check
function test(value = 'default') {
  return value ?? 'default';
}
```

## Related Topics

- [[Destructure-Objects]]
- [[Use-Spread]]
- [[Use-LetConst]]

## Quick Revision

| Syntax | Purpose |
|--------|---------|
| `function(a = 1)` | Default parameter |
| `({ a = 1 } = {})` | Destructure with defaults |
| `undefined` triggers default | Not null |
| Use for | Optional arguments, config |
