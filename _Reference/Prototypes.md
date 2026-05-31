# Prototypes in JavaScript

## Definition

Prototypes are the mechanism by which JavaScript objects inherit features from one another. Every object has an internal link to another object called its prototype, forming a prototype chain that enables property and method inheritance.

## What is a Prototype?

A prototype is an object from which other objects inherit properties and methods. When you access a property on an object, JavaScript first looks on the object itself, then traverses up the prototype chain until it finds the property or reaches `null`.

## Code Examples

### Prototype Chain

```javascript
const animal = {
  eats: true,
  walk() {
    return "Walking...";
  }
};

const dog = Object.create(animal);
dog.barks = true;

console.log(dog.barks);    // true (own property)
console.log(dog.eats);     // true (inherited from animal)
console.log(dog.walk());   // "Walking..." (inherited method)
```

### Constructor Functions and Prototypes

```javascript
function Person(name, age) {
  this.name = name;
  this.age = age;
}

Person.prototype.greet = function() {
  return `Hi, I'm ${this.name}`;
};

const john = new Person("John", 30);
const jane = new Person("Jane", 25);

console.log(john.greet()); // "Hi, I'm John"
console.log(jane.greet()); // "Hi, I'm Jane"

// Both share the same prototype method
console.log(john.greet === jane.greet); // true
```

### ES6 Classes (Syntactic Sugar over Prototypes)

```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }

  speak() {
    return `${this.name} makes a sound`;
  }
}

class Dog extends Animal {
  speak() {
    return `${this.name} barks`;
  }
}

const rex = new Dog("Rex");
console.log(rex.speak()); // "Rex barks"
```

### Prototype Inheritance

```javascript
function Vehicle(type) {
  this.type = type;
}

Vehicle.prototype.start = function() {
  return `${this.type} starting...`;
};

function Car(brand) {
  Vehicle.call(this, "Car");
  this.brand = brand;
}

// Set up prototype chain
Car.prototype = Object.create(Vehicle.prototype);
Car.prototype.constructor = Car;

Car.prototype.honk = function() {
  return `${this.brand} says beep!`;
};

const myCar = new Car("Toyota");
console.log(myCar.start()); // "Car starting..."
console.log(myCar.honk());  // "Toyota says beep!"
```

### Checking Prototypes

```javascript
const arr = [1, 2, 3];

console.log(Array.prototype);          // Array methods
console.log(Object.getPrototypeOf(arr) === Array.prototype); // true
console.log(arr instanceof Array);     // true
console.log("push" in arr);            // true (inherited)
console.log(arr.hasOwnProperty("push")); // false (not own)
```

## Common Use Cases

- Creating reusable blueprints for objects
- Implementing inheritance hierarchies
- Adding methods to built-in objects (though not recommended)
- Sharing methods across instances to save memory
- Implementing private-like patterns with closures

## Common Mistakes

1. **Forgetting `new` keyword**: Constructor functions without `new` modify global object

```javascript
function Person(name) {
  this.name = name;
}

// Without new - modifies global scope (in non-strict mode)
const john = Person("John"); // window.name = "John" (browser)
```

2. **Overwriting prototypes**: Can break existing code

```javascript
// Bad - overwrites entire prototype
Person.prototype = {
  greet() { return "Hi"; }
};

// Better - extend existing prototype
Person.prototype.greet = function() { return "Hi"; };
```

3. **Modifying native prototypes**: Can cause unpredictable behavior

```javascript
// Bad practice
Array.prototype.myMethod = function() { /* ... */ };
```

## Related Topics

- [[Classes]] - ES6 class syntax
- [[Inheritance]] - Object inheritance patterns
- [[Object-Create]] - Creating objects with specific prototypes
- [[This-Keyword]] - How `this` works in prototype methods
- [[Constructor-Functions]] - Creating objects with constructors
- [[OOP-Concepts]] - Object-oriented programming in JavaScript

## Quick Revision Summary

| Concept | Description |
|---------|-------------|
| Prototype | Object from which other objects inherit |
| Prototype Chain | Link of objects from instance to null |
| `__proto__` | Legacy way to access prototype (use `Object.getPrototypeOf()`) |
| `Object.create()` | Creates object with specified prototype |
| Classes | ES6 syntax sugar over prototype-based inheritance |
| Memory | Shared methods save memory via prototype |
