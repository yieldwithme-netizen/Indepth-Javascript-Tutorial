# Defining Objects

## Definition

Objects in JavaScript are collections of key-value pairs where values can be any data type (functions, arrays, other objects). They are the fundamental building blocks for creating complex data structures and are used to model real-world entities and organize related data and behavior.

## Code Examples

### Object Literal

```javascript
const person = {
  firstName: "Alice",
  lastName: "Smith",
  age: 30,
  greet: function () {
    return `Hello, I'm ${this.firstName} ${this.lastName}`;
  },
};

console.log(person.firstName);      // "Alice"
console.log(person.greet());        // "Hello, I'm Alice Smith"
```

### Constructor Function

```javascript
function Car(make, model, year) {
  this.make = make;
  this.model = model;
  this.year = year;
  this.start = function () {
    console.log(`${this.make} ${this.model} started`);
  };
}

const car1 = new Car("Toyota", "Camry", 2024);
car1.start(); // "Toyota Camry started"
```

### Class Syntax (ES6)

```javascript
class Animal {
  constructor(name, type) {
    this.name = name;
    this.type = type;
  }

  speak() {
    return `${this.name} makes a sound`;
  }

  static create(name, type) {
    return new Animal(name, type);
  }
}

const dog = new Animal("Rex", "Dog");
console.log(dog.speak()); // "Rex makes a sound"

const cat = Animal.create("Whiskers", "Cat");
```

### Factory Function

```javascript
function createUser(name, email) {
  return {
    name,
    email,
    greet() {
      return `Hi, I'm ${this.name}`;
    },
  };
}

const user = createUser("Alice", "alice@example.com");
console.log(user.greet()); // "Hi, I'm Alice"
```

### Object.create()

```javascript
const proto = {
  greet() {
    return `Hello, ${this.name}`;
  },
};

const obj = Object.create(proto);
obj.name = "Bob";
console.log(obj.greet()); // "Hello, Bob"
```

### Computed Property Names

```javascript
const prop = "age";
const person = {
  name: "Alice",
  [prop]: 30,
  [`get${prop.charAt(0).toUpperCase() + prop.slice(1)}`]() {
    return this[prop];
  },
};

console.log(person.age);           // 30
console.log(person.getAge());      // 30
```

### Getters and Setters

```javascript
class Temperature {
  #celsius;

  constructor(celsius) {
    this.#celsius = celsius;
  }

  get fahrenheit() {
    return this.#celsius * 1.8 + 32;
  }

  set fahrenheit(f) {
    this.#celsius = (f - 32) / 1.8;
  }
}

const temp = new Temperature(100);
console.log(temp.fahrenheit);  // 212
temp.fahrenheit = 32;
console.log(temp.fahrenheit);  // 32
```

### Object Destructuring

```javascript
const { name, age, ...rest } = {
  name: "Alice",
  age: 30,
  city: "NYC",
  country: "USA",
};

console.log(name);     // "Alice"
console.log(rest);     // { city: "NYC", country: "USA" }
```

## Common Use Cases

- **Data modeling** — Represent entities like users, products, orders
- **Configuration objects** — Store app settings
- **API responses** — Parse and structure data
- **State management** — Store application state
- **Mixins** — Combine behavior from multiple sources
- **Caching** — Use objects as hash maps

## Common Mistakes

```javascript
// Mistake 1: Using reserved words as keys
// const obj = { class: "foo" }; // Works but bad practice
const obj = { className: "foo" }; // Better

// Mistake 2: Not using 'this' in methods
const person = {
  name: "Alice",
  greet() {
    // return `Hello, ${name}`; // ReferenceError
    return `Hello, ${this.name}`; // Correct
  },
};

// Mistake 3: Using '==' for object comparison
const a = { x: 1 };
const b = { x: 1 };
console.log(a == b);   // false (different references)
console.log(a === b);  // false

// Mistake 4: Mutating shared objects
const defaults = { color: "red", size: 10 };
// const userConfig = defaults; // Mutates defaults!
const userConfig = { ...defaults }; // Safe copy
```

## Related Topics

- [[Object-Methods]]
- [[Object-Destructuring]]
- [[Classes]]
- [[Prototypes]]
- [[Getters-Setters]]
- [[This-Keyword]]
- [[Spread-Operator]]

## Quick Revision

| Method | Syntax | Use Case |
|--------|--------|----------|
| Literal | `{}` | Quick object creation |
| Constructor | `new Func()` | Reusable with `new` |
| Class | `class {}` | Modern, with inheritance |
| Factory | `function()` | Flexible, no `new` needed |
| `Object.create()` | `Object.create(proto)` | Prototype-based creation |

| Feature | Description |
|---------|-------------|
| Keys | Strings or Symbols |
| Values | Any data type |
| Methods | Functions as values |
| `this` | Refers to the object |
| Spread | `{...obj}` for shallow copy |
