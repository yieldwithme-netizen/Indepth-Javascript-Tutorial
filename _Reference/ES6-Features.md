# ES6 Features

## Definition

ES6 (ECMAScript 2015) introduced significant enhancements to JavaScript, making the language more powerful, expressive, and developer-friendly. These features include arrow functions, template literals, destructuring, modules, classes, and more. ES6+ refers to all modern JavaScript features from ES2015 onwards.

## Code Examples

### Arrow Functions

```javascript
// ES5
var multiply = function(a, b) {
  return a * b;
};

// ES6
const multiply = (a, b) => a * b;

// With block body
const divide = (a, b) => {
  const result = a / b;
  return result;
};
```

### Template Literals

```javascript
const name = "Alice";
const age = 25;

// String concatenation (ES5)
const greeting1 = "Hello, " + name + ". You are " + age + " years old.";

// Template literals (ES6)
const greeting2 = `Hello, ${name}. You are ${age} years old.`;

// Multi-line strings
const multiLine = `
  Line 1
  Line 2
  Line 3
`;

// Expression interpolation
const price = 19.99;
const message = `Total: $${(price * 1.1).toFixed(2)}`;
```

### Destructuring Assignment

```javascript
// Object destructuring
const person = { name: "Bob", age: 30, city: "NYC" };
const { name, age, city } = person;

// Array destructuring
const colors = ["red", "green", "blue"];
const [first, second, third] = colors;

// Default values
const { name: userName, role = "user" } = person;

// Nested destructuring
const user = {
  profile: {
    name: "Charlie",
    address: {
      city: "LA",
      state: "CA"
    }
  }
};
const { profile: { address: { city, state } } } = user;
```

### Spread and Rest Operators

```javascript
// Spread operator - expanding arrays/objects
const arr1 = [1, 2, 3];
const arr2 = [...arr1, 4, 5]; // [1, 2, 3, 4, 5]

const obj1 = { a: 1, b: 2 };
const obj2 = { ...obj1, c: 3 }; // { a: 1, b: 2, c: 3 }

// Rest parameters - collecting arguments
function sum(...numbers) {
  return numbers.reduce((total, num) => total + num, 0);
}
sum(1, 2, 3, 4); // 10
```

### Classes

```javascript
class Animal {
  constructor(name, sound) {
    this.name = name;
    this.sound = sound;
  }

  speak() {
    return `${this.name} says ${this.sound}!`;
  }

  static create(name, sound) {
    return new Animal(name, sound);
  }
}

class Dog extends Animal {
  constructor(name) {
    super(name, "Woof");
  }

  fetch(item) {
    return `${this.name} fetches the ${item}`;
  }
}

const dog = new Dog("Rex");
console.log(dog.speak()); // "Rex says Woof!"
console.log(dog.fetch("ball")); // "Rex fetches the ball"
```

### Promises

```javascript
// Creating a promise
function fetchData(url) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve({ data: "response" });
    }, 1000);
  });
}

// Using promises
fetchData("/api/data")
  .then(response => console.log(response))
  .catch(error => console.error(error))
  .finally(() => console.log("Done"));
```

### Let and Const

```javascript
// var - function scoped, can be redeclared
var x = 10;
var x = 20; // Works

// let - block scoped, can be reassigned
let y = 10;
y = 20; // Works
// let y = 30; // Error: already declared

// const - block scoped, cannot be reassigned
const z = 10;
// z = 20; // Error: assignment to constant
```

### Default Parameters

```javascript
function greet(name = "Guest", greeting = "Hello") {
  return `${greeting}, ${name}!`;
}

greet();                    // "Hello, Guest!"
greet("Alice");            // "Hello, Alice!"
greet("Bob", "Hi");       // "Hi, Bob!"
```

### Array Methods

```javascript
const numbers = [1, 2, 3, 4, 5];

// Map - transform elements
const doubled = numbers.map(n => n * 2); // [2, 4, 6, 8, 10]

// Filter - select elements
const evens = numbers.filter(n => n % 2 === 0); // [2, 4]

// Reduce - accumulate values
const sum = numbers.reduce((acc, n) => acc + n, 0); // 15

// Find - get first match
const found = numbers.find(n => n > 3); // 4

// Some - check if any match
const hasEven = numbers.some(n => n % 2 === 0); // true

// Every - check if all match
const allPositive = numbers.every(n => n > 0); // true
```

### Optional Chaining (ES2020)

```javascript
const user = {
  profile: {
    name: "Alice",
    address: {
      city: "Seattle"
    }
  }
};

// Without optional chaining
const city1 = user && user.profile && user.profile.address && user.profile.address.city;

// With optional chaining
const city2 = user?.profile?.address?.city; // "Seattle"
const zip = user?.profile?.address?.zip; // undefined
```

### Nullish Coalescing (ES2020)

```javascript
const value1 = null ?? "default"; // "default"
const value2 = undefined ?? "default"; // "default"
const value3 = 0 ?? "default"; // 0
const value4 = "" ?? "default"; // ""
```

### Map and Set

```javascript
// Map - key-value pairs
const map = new Map();
map.set("name", "Alice");
map.set(42, "answer");
console.log(map.get("name")); // "Alice"
console.log(map.size); // 2

// Set - unique values
const set = new Set([1, 2, 2, 3, 3, 3]);
console.log(set.size); // 3
console.log(set.has(2)); // true
```

## Common Use Cases

1. **Modern syntax** - Write cleaner, more concise code
2. **Functional programming** - Use higher-order functions
3. **Async handling** - Manage asynchronous operations
4. **Data structures** - Use Map, Set for specialized collections
5. **Class-based code** - Create object-oriented structures

## Common Mistakes

1. **Using `var` in loops** - Use `let` for block scoping
2. **Not handling promises** - Always add `.catch()` or use try/catch
3. **Mutating const objects** - `const` prevents reassignment, not mutation
4. **Arrow functions in methods** - They don't have their own `this`

```javascript
// WRONG: var in loop
for (var i = 0; i < 5; i++) {
  setTimeout(() => console.log(i), 100); // Logs 5 five times
}

// RIGHT: let in loop
for (let i = 0; i < 5; i++) {
  setTimeout(() => console.log(i), 100); // Logs 0, 1, 2, 3, 4
}
```

## Quick Revision Summary

- Arrow functions provide concise syntax and lexical `this`
- Template literals enable string interpolation and multi-line strings
- Destructuring extracts values from arrays/objects
- Spread expands, rest collects
- Classes provide OOP syntax
- Promises handle async operations
- `let` and `const` replace `var`
- Optional chaining and nullish coalescing handle null/undefined safely

## Related Topics

- [[Arrow-Functions]]
- [[Destructuring-Assignment]]
- [[Template-Literals]]
- [[Promises]]
- [[Classes]]
- [[Modules]]
- [[Spread-Operator]]
- [[Rest-Parameters]]
