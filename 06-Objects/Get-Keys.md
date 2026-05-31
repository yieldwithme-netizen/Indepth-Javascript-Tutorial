# How to Use Object.keys()

`Object.keys()` returns an array of a given object's **own enumerable property names**.

## Basic Syntax

```javascript
Object.keys(object);
```

## Basic Examples

```javascript
const person = {
  name: "John",
  age: 30,
  city: "New York"
};

const keys = Object.keys(person);
console.log(keys); // ["name", "age", "city"]
```

## Iterating Over Keys

```javascript
const car = {
  make: "Toyota",
  model: "Camry",
  year: 2020
};

// Using for...of
for (const key of Object.keys(car)) {
  console.log(`${key}: ${car[key]}`);
}

// Using forEach
Object.keys(car).forEach(key => {
  console.log(`${key}: ${car[key]}`);
});
```

## Counting Properties

```javascript
const user = {
  name: "Alice",
  email: "alice@example.com",
  age: 25
};

console.log(Object.keys(user).length); // 3
```

## Filtering Keys

```javascript
const data = {
  name: "John",
  age: 30,
  active: true
};

const activeKeys = Object.keys(data).filter(key => data[key] === true);
console.log(activeKeys); // ["active"]
```

## Common Use Cases

- Iterating over object properties
- Counting object properties
- Checking if property exists
- Converting objects to arrays

## Common Mistakes

```javascript
const obj = { a: 1, b: 2 };

// ❌ Using on non-objects
// Object.keys("string"); // TypeError
// Object.keys(123);      // TypeError

// ✅ Check if object before calling
const isObject = (val) => typeof val === 'object' && val !== null;
if (isObject(obj)) {
  console.log(Object.keys(obj));
}

// ❌ Expecting non-enumerable properties
const hidden = {};
Object.defineProperty(hidden, 'secret', {
  value: 42,
  enumerable: false
});
console.log(Object.keys(hidden)); // [] (secret not included)
```

## Related Topics

- [[Get-Values]]
- [[Iterate-Objects]]
- [[Define-Objects]]
- [[Object-Entries]]

## Quick Revision

| Method | Returns | Example |
|--------|---------|---------|
| `Object.keys(obj)` | Array of keys | `["name", "age"]` |
| `Object.values(obj)` | Array of values | `["John", 30]` |
| `Object.entries(obj)` | Array of [key, value] pairs | `[["name", "John"]]` |

**Key Point:** `Object.keys()` returns an array, so you can use all array methods on it.