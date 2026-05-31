# Destructuring

Destructuring is an ES6 feature that allows you to extract values from objects and arrays and assign them to variables in a concise syntax. It makes code more readable and expressive.

## Object Destructuring

### Basic Syntax

```javascript
// Old way
const person = { name: 'Alice', age: 25 };
const name = person.name;
const age = person.age;

// Destructuring
const { name, age } = person;
console.log(name); // 'Alice'
console.log(age);  // 25
```

### Renaming Variables

```javascript
const person = { name: 'Alice', age: 25 };

// Rename while destructuring
const { name: personName, age: personAge } = person;
console.log(personName); // 'Alice'
console.log(personAge);  // 25
```

### Default Values

```javascript
const person = { name: 'Alice' };

// Set defaults for missing properties
const { name, age = 0, email = 'N/A' } = person;
console.log(name);  // 'Alice'
console.log(age);   // 0
console.log(email); // 'N/A'
```

### Rest Pattern

```javascript
const person = { name: 'Alice', age: 25, email: 'alice@email.com' };

// Extract some properties, rest as new object
const { name, ...details } = person;
console.log(name);    // 'Alice'
console.log(details); // { age: 25, email: 'alice@email.com' }
```

### Nested Objects

```javascript
const user = {
  id: 1,
  name: 'Alice',
  address: {
    street: '123 Main St',
    city: 'New York',
    zip: '10001'
  }
};

// Nested destructuring
const { name, address: { city, zip } } = user;
console.log(name); // 'Alice'
console.log(city); // 'New York'
console.log(zip);  // '10001'

// With defaults in nested objects
const { address: { phone = 'N/A' } = {} } = user;
console.log(phone); // 'N/A'
```

## Array Destructuring

### Basic Syntax

```javascript
const colors = ['red', 'green', 'blue'];
const [first, second, third] = colors;
console.log(first);  // 'red'
console.log(second); // 'green'
console.log(third);  // 'blue'
```

### Skipping Elements

```javascript
const colors = ['red', 'green', 'blue', 'yellow'];
const [first, , third] = colors;
console.log(first); // 'red'
console.log(third); // 'blue'
```

### Rest Pattern

```javascript
const numbers = [1, 2, 3, 4, 5];
const [first, second, ...rest] = numbers;
console.log(first);  // 1
console.log(second); // 2
console.log(rest);   // [3, 4, 5]
```

## Function Parameters

### Object Parameters

```javascript
// Without destructuring
function createUser(user) {
  const name = user.name;
  const age = user.age;
  const role = user.role || 'user';
  return { name, age, role };
}

// With destructuring
function createUser({ name, age, role = 'user' }) {
  return { name, age, role };
}

createUser({ name: 'Alice', age: 25 });
```

### Array Parameters

```javascript
function usePosition([x = 0, y = 0] = []) {
  return { x, y };
}

usePosition([10, 20]); // { x: 10, y: 20 }
usePosition();         // { x: 0, y: 0 }
```

## Advanced Patterns

### Combined Object and Array

```javascript
const response = {
  data: {
    users: [
      { id: 1, name: 'Alice' },
      { id: 2, name: 'Bob' }
    ],
    total: 2
  },
  status: 200
};

const { data: { users: [firstUser, secondUser], total }, status } = response;
console.log(firstUser); // { id: 1, name: 'Alice' }
console.log(total);     // 2
console.log(status);    // 200
```

### Computed Property Names

```javascript
const key = 'name';
const { [key]: value } = { name: 'Alice' };
console.log(value); // 'Alice'
```

### Destructuring with Map and Filter

```javascript
const users = [
  { name: 'Alice', age: 25 },
  { name: 'Bob', age: 30 },
  { name: 'Charlie', age: 35 }
];

// Extract names
const names = users.map(({ name }) => name);

// Filter by age
const adults = users.filter(({ age }) => age >= 30);
```

## Common Use Cases

- API response handling
- Configuration objects
- React hooks
- Module imports
- Iterating collections

## Common Mistakes

1. **Over-destructuring** - Don't extract everything you don't use
2. **Deep nesting** - Keep it readable (max 2-3 levels)
3. **Missing defaults** - Set defaults for optional properties
4. **Mutating variables** - Destructuring doesn't mutate original
5. **Order matters** - Array destructuring depends on position

## Performance Considerations

```javascript
// Good: Destructure once
const { name, age, email } = user;
process(name, age, email);

// Bad: Destructure multiple times
process(user.name, user.age, user.email);
```

## Related Topics

- [[Spread-Operator]]
- [[Rest-Parameters]]
- [[Arrow-Functions]]
- [[Array-Methods]]
- [[ES6-Features]]

## Quick Revision

| Type | Syntax | Use Case |
|------|--------|----------|
| Object | `{ a, b } = obj` | Extract properties |
| Array | `[a, b] = arr` | Extract by position |
| Rename | `{ a: x } = obj` | Rename variables |
| Default | `{ a = 1 } = obj` | Fallback values |
| Rest | `{ a, ...rest } = obj` | Collect remaining |
| Nested | `{ a: { b } } = obj` | Deep extraction |

Destructuring is one of ES6's most useful features, making code more concise and readable when working with complex data structures.