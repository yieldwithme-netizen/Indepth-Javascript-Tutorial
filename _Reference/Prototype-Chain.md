# Prototype Chain in JavaScript

## Definition

The **prototype chain** is JavaScript's mechanism for object inheritance. Every object has an internal link to another object called its **prototype**. When you access a property that doesn't exist on an object, JavaScript walks up the chain until it finds the property or reaches `null`. This is the foundation of JavaScript's object-oriented programming.

---

## How Prototypes Work

```javascript
// Every object has a prototype
const obj = {};
console.log(Object.getPrototypeOf(obj)); // Object.prototype

// Functions have a prototype property
function Person(name) {
  this.name = name;
}
console.log(Person.prototype); // { constructor: Person }

// Instances are linked to constructor's prototype
const alice = new Person("Alice");
console.log(Object.getPrototypeOf(alice) === Person.prototype); // true
```

---

## The Prototype Chain

```javascript
function Animal(type) {
  this.type = type;
}

Animal.prototype.speak = function() {
  return `${this.type} makes a sound`;
};

function Dog(name) {
  Animal.call(this, "Dog");
  this.name = name;
}

// Set up inheritance
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;

Dog.prototype.bark = function() {
  return `${this.name} barks!`;
};

const rex = new Dog("Rex");

// Chain: rex -> Dog.prototype -> Animal.prototype -> Object.prototype -> null
console.log(rex.bark()); // "Rex barks!" (found on Dog.prototype)
console.log(rex.speak()); // "Dog makes a sound" (found on Animal.prototype)
console.log(rex.toString()); // "[object Object]" (found on Object.prototype)
```

---

## Property Lookup

```javascript
const animal = { eats: true };
const rabbit = { jumps: true };

// Set prototype chain
Object.setPrototypeOf(rabbit, animal);

console.log(rabbit.jumps); // true (own property)
console.log(rabbit.eats); // true (inherited from animal)
console.log(rabbit.walks); // undefined (not found in chain)

// hasOwnProperty only checks own properties
console.log(rabbit.hasOwnProperty("jumps")); // true
console.log(rabbit.hasOwnProperty("eats")); // false
```

---

## Creating Prototypal Inheritance

### Constructor Functions

```javascript
function Vehicle(make, model) {
  this.make = make;
  this.model = model;
}

Vehicle.prototype.describe = function() {
  return `${this.make} ${this.model}`;
};

function Car(make, model, doors) {
  Vehicle.call(this, make, model);
  this.doors = doors;
}

Car.prototype = Object.create(Vehicle.prototype);
Car.prototype.constructor = Car;

Car.prototype.getDoors = function() {
  return this.doors;
};

const myCar = new Car("Toyota", "Camry", 4);
console.log(myCar.describe()); // "Toyota Camry"
console.log(myCar.getDoors()); // 4
```

### ES6 Classes

```javascript
class Animal {
  constructor(type) {
    this.type = type;
  }
  
  speak() {
    return `${this.type} makes a sound`;
  }
}

class Dog extends Animal {
  constructor(name) {
    super("Dog");
    this.name = name;
  }
  
  bark() {
    return `${this.name} barks!`;
  }
}

const rex = new Dog("Rex");
console.log(rex.bark()); // "Rex barks!"
console.log(rex.speak()); // "Dog makes a sound"
```

### Object.create()

```javascript
const personMethods = {
  greet() {
    return `Hello, I'm ${this.name}`;
  },
  goodbye() {
    return `Goodbye from ${this.name}`;
  }
};

function createPerson(name) {
  const person = Object.create(personMethods);
  person.name = name;
  return person;
}

const alice = createPerson("Alice");
console.log(alice.greet()); // "Hello, I'm Alice"
console.log(Object.getPrototypeOf(alice) === personMethods); // true
```

---

## Prototype vs __proto__

```javascript
const obj = {};

// __proto__ is the actual link (legacy but widely supported)
console.log(obj.__proto__ === Object.prototype); // true

// Object.getPrototypeOf() is the recommended way
console.log(Object.getPrototypeOf(obj) === Object.prototype); // true

// Setting prototypes
const proto = { greeting: "Hello" };
const obj2 = Object.create(proto);
console.log(obj2.greeting); // "Hello"

// Modern way to set prototype
Object.setPrototypeOf(obj2, { greeting: "Hi" });
console.log(obj2.greeting); // "Hi"
```

---

## Common Use Cases

### Method Inheritance

```javascript
class Shape {
  constructor(color) {
    this.color = color;
  }
  
  describe() {
    return `A ${this.color} shape`;
  }
}

class Circle extends Shape {
  constructor(color, radius) {
    super(color);
    this.radius = radius;
  }
  
  area() {
    return Math.PI * this.radius ** 2;
  }
  
  describe() {
    return `A ${this.color} circle`;
  }
}

const redCircle = new Circle("red", 5);
console.log(redCircle.describe()); // "A red circle" (Circle's method)
console.log(redCircle.area()); // 78.539... (Circle's method)
```

### Mixins

```javascript
const Serializable = {
  serialize() {
    return JSON.stringify(this);
  },
  
  deserialize(json) {
    return Object.assign(this, JSON.parse(json));
  }
};

const Loggable = {
  log() {
    console.log(`${this.constructor.name}:`, this);
  }
};

class User {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }
}

// Apply mixins
Object.assign(User.prototype, Serializable, Loggable);

const user = new User("Alice", "alice@example.com");
console.log(user.serialize()); // '{"name":"Alice","email":"alice@example.com"}'
user.log(); // User: { name: "Alice", email: "alice@example.com" }
```

### Built-in Prototype Extension

```javascript
// Extend Array (use with caution!)
Array.prototype.last = function() {
  return this[this.length - 1];
};

const arr = [1, 2, 3];
console.log(arr.last()); // 3

// Better: use a utility function
const last = arr => arr[arr.length - 1];
console.log(last([1, 2, 3])); // 3
```

---

## Common Mistakes

### Mistake 1: Modifying Prototype Directly

```javascript
// Wrong: affects all instances
Array.prototype.myMethod = function() { /* ... */ };

// Better: extend specific class
class MyArray extends Array {
  myMethod() { /* ... */ }
}
```

### Mistake 2: Forgetting to Call super()

```javascript
class Parent {
  constructor(name) {
    this.name = name;
  }
}

class Child extends Parent {
  constructor(name, age) {
    // super() must be called before using 'this'
    super(name);
    this.age = age;
  }
}

// Wrong: using this before super()
class Child extends Parent {
  constructor(name, age) {
    this.age = age; // ReferenceError!
    super(name);
  }
}
```

### Mistake 3: Confusing prototype with __proto__

```javascript
const obj = {};

// prototype is a property of functions
console.log(obj.prototype); // undefined

// __proto__ is the actual prototype link
console.log(obj.__proto__); // Object.prototype

// Use Object.getPrototypeOf() instead
console.log(Object.getPrototypeOf(obj)); // Object.prototype
```

### Mistake 4: Circular Prototypes

```javascript
const a = {};
const b = Object.create(a);
Object.setPrototypeOf(a, b); // Circular! 

// This causes infinite loop
console.log(a.x); // Stack overflow
```

---

## Quick Revision Summary

| Concept | Description |
|---------|-------------|
| `__proto__` | Actual prototype link |
| `prototype` | Property of functions |
| `Object.getPrototypeOf()` | Get prototype safely |
| `Object.setPrototypeOf()` | Set prototype |
| `Object.create()` | Create with prototype |
| `hasOwnProperty()` | Check own properties |
| `in` operator | Check chain (including inherited) |

---

## Related Topics

- [[Prototype-Chain]] - This file
- [[Prototypes]] - Prototype-based inheritance
- [[class]] - ES6 classes (syntactic sugar)
- [[this]] - `this` context in prototypes
- [[Object]] - Object fundamentals
- [[Object-Methods]] - Prototype methods
- [[Symbol-Iterator]] - Iterator protocol on prototypes