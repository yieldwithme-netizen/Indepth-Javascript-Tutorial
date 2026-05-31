# Destructuring

## Definition
Destructuring is a JavaScript syntax that unpacks values from arrays or properties from objects into distinct variables, making it easier to work with complex data structures.

## Array Destructuring

### Basic Syntax
```javascript
// Without destructuring
const arr = [1, 2, 3];
const a = arr[0];
const b = arr[1];
const c = arr[2];

// With destructuring
const [x, y, z] = [1, 2, 3];
console.log(x);  // 1
console.log(y);  // 2
console.log(z);  // 3
```

### Skipping Elements
```javascript
const [first, , third] = [1, 2, 3];
console.log(first);  // 1
console.log(third);  // 3
```

### Default Values
```javascript
const [a = 10, b = 20, c = 30] = [1, 2];
console.log(a);  // 1
console.log(b);  // 2
console.log(c);  // 30 (used default)
```

### Rest Pattern
```javascript
const [head, ...tail] = [1, 2, 3, 4, 5];
console.log(head);  // 1
console.log(tail);  // [2, 3, 4, 5]
```

### Swapping Variables
```javascript
let a = 1;
let b = 2;
[a, b] = [b, a];
console.log(a);  // 2
console.log(b);  // 1
```

## Object Destructuring

### Basic Syntax
```javascript
// Without destructuring
const obj = { name: "John", age: 25, city: "NYC" };
const name = obj.name;
const age = obj.age;

// With destructuring
const { name, age, city } = { name: "John", age: 25, city: "NYC" };
console.log(name);  // "John"
console.log(age);   // 25
```

### Renaming Variables
```javascript
const { name: userName, age: userAge } = { name: "John", age: 25 };
console.log(userName);  // "John"
console.log(userAge);   // 25
```

### Default Values
```javascript
const { name, age, role = "user" } = { name: "John", age: 25 };
console.log(role);  // "user" (used default)
```

### Nested Destructuring
```javascript
const user = {
  name: "John",
  address: {
    street: "123 Main St",
    city: "NYC",
    state: "NY"
  }
};

const { name, address: { street, city } } = user;
console.log(name);    // "John"
console.log(street);  // "123 Main St"
console.log(city);    // "NYC"
```

### Rest Pattern
```javascript
const { name, ...rest } = { name: "John", age: 25, city: "NYC" };
console.log(name);  // "John"
console.log(rest);  // { age: 25, city: "NYC" }
```

## Function Parameters Destructuring

```javascript
// Array destructuring in function params
function printCoordinates([x, y, z]) {
  console.log(`X: ${x}, Y: ${y}, Z: ${z}`);
}
printCoordinates([10, 20, 30]);

// Object destructuring in function params
function createUser({ name, age, role = "user" }) {
  return { name, age, role };
}
const user = createUser({ name: "John", age: 25 });

// Combined with defaults
function greet({ name = "Guest", greeting = "Hello" } = {}) {
  return `${greeting}, ${name}!`;
}
console.log(greet());                    // "Hello, Guest!"
console.log(greet({ name: "John" }));   // "Hello, John!"
```

## Common Use Cases
- Extracting data from API responses
- Function parameter handling
- Swapping variables
- Working with array methods (map, filter)
- Importing specific exports

## Common Mistakes

| Mistake | Solution |
|---------|----------|
| Not providing defaults | Use `=` for fallback values |
| Over-nesting | Keep destructuring shallow |
| Missing variables | Variables must exist or use defaults |
| Forgetting rename syntax | Use `:` to rename |

## Quick Revision Summary
- Array destructuring uses `[]` bracket syntax
- Object destructuring uses `{}` brace syntax
- Supports defaults, renaming, and rest patterns
- Works in function parameters
- Simplifies working with complex data structures
- Makes code more readable and concise

## Related Topics
- [[Spread-Operator]]
- [[Rest-Parameters]]
- [[Arrow-Functions]]
- [[Array-Methods]]
- [[Object-Methods]]
- [[Template-Literals]]
