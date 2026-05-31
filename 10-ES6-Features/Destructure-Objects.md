# Destructure Objects

## Definition

Destructuring is an ES6 syntax that extracts values from objects or arrays into distinct variables. It provides a concise way to unpack data structures.

## Code Examples

### Basic Object Destructuring

```javascript
const person = {
  name: 'John',
  age: 30,
  city: 'New York'
};

// Without destructuring
const name = person.name;
const age = person.age;

// With destructuring
const { name, age } = person;
console.log(name, age); // John 30
```

### Rename Variables

```javascript
const { name: personName, age: personAge } = person;
console.log(personName, personAge);
```

### Default Values

```javascript
const { name, age, country = 'USA' } = person;
console.log(country); // USA (default value)
```

### Nested Destructuring

```javascript
const user = {
  id: 1,
  profile: {
    name: 'John',
    address: {
      city: 'New York',
      zip: '10001'
    }
  }
};

const { profile: { name, address: { city } } } = user;
console.log(name, city); // John New York
```

### Function Parameters

```javascript
function greet({ name, age }) {
  console.log(`Hello ${name}, you are ${age} years old`);
}

greet({ name: 'John', age: 30 });
```

### Rest Operator

```javascript
const { name, ...rest } = person;
console.log(rest); // { age: 30, city: 'New York' }
```

### Array Destructuring

```javascript
const numbers = [1, 2, 3, 4, 5];

const [first, second, ...rest] = numbers;
console.log(first, second); // 1 2
console.log(rest); // [3, 4, 5]
```

### Skip Elements

```javascript
const [first, , third] = numbers;
console.log(first, third); // 1 3
```

## Common Use Cases

1. **API responses** - Extract specific fields
2. **Function parameters** - Pass objects as arguments
3. **React props** - Destructure component props
4. **Module imports** - Import specific exports

## Common Mistakes

```javascript
// Wrong: Not handling undefined values
const { name, address } = user;
// address may be undefined

// Correct: Use default values
const { name, address = {} } = user;

// Wrong: Over-nesting
const { a: { b: { c: { d } } } } = obj; // Too deep

// Better: Break into multiple lines
const { a } = obj;
const { b } = a;
const { c } = b;
const { d } = c;
```

## Related Topics

- [[Use-Spread]]
- [[Set-Defaults]]
- [[Use-LetConst]]
- [[Handle-Form]]

## Quick Revision

| Syntax | Purpose |
|--------|---------|
| `const { a } = obj` | Extract property `a` |
| `const { a: b } = obj` | Rename to `b` |
| `const { a = 1 } = obj` | Default value |
| `const { a, ...rest } = obj` | Rest properties |
| `const [a, b] = arr` | Array destructuring |
