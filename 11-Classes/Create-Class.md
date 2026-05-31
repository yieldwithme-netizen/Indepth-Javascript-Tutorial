# How to Create Classes in JavaScript

## Definition

A class is a blueprint for creating objects with predefined properties and methods. JavaScript classes provide a cleaner, more structured syntax for creating and managing objects using the `class` keyword, introduced in ES6.

## Syntax

```javascript
class ClassName {
  constructor(parameters) {
    // Initialize properties
  }

  // Methods
  methodName() {
    // Method body
  }
}
```

## Code Examples

### Basic Class

```javascript
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  greet() {
    return `Hello, my name is ${this.name} and I am ${this.age} years old.`;
  }
}

const john = new Person('John', 30);
console.log(john.greet()); // "Hello, my name is John and I am 30 years old."
```

### Class with Default Values

```javascript
class Car {
  constructor(make = 'Unknown', model = 'Unknown', year = 2024) {
    this.make = make;
    this.model = model;
    this.year = year;
    this.isRunning = false;
  }

  start() {
    this.isRunning = true;
    return `${this.make} ${this.model} started.`;
  }

  stop() {
    this.isRunning = false;
    return `${this.make} ${this.model} stopped.`;
  }
}

const myCar = new Car('Toyota', 'Camry');
console.log(myCar.start()); // "Toyota Camry started."
```

### Class with Getters and Setters

```javascript
class Temperature {
  constructor(celsius = 0) {
    this._celsius = celsius;
  }

  get fahrenheit() {
    return (this._celsius * 9/5) + 32;
  }

  set fahrenheit(f) {
    this._celsius = (f - 32) * 5/9;
  }

  get celsius() {
    return this._celsius;
  }

  set celsius(c) {
    this._celsius = c;
  }
}

const temp = new Temperature(100);
console.log(temp.fahrenheit); // 212
temp.fahrenheit = 32;
console.log(temp.celsius); // 0
```

### Static Methods

```javascript
class MathHelper {
  static add(a, b) {
    return a + b;
  }

  static multiply(a, b) {
    return a * b;
  }

  static factorial(n) {
    if (n <= 1) return 1;
    return n * MathHelper.factorial(n - 1);
  }
}

console.log(MathHelper.add(5, 3)); // 8
console.log(MathHelper.factorial(5)); // 120
```

## Common Use Cases

| Use Case | Example |
|----------|---------|
| Data Modeling | `User`, `Product`, `Order` classes |
| UI Components | `Button`, `Modal`, `Form` classes |
| Game Development | `Player`, `Enemy`, `Projectile` classes |
| Service Layer | `APIService`, `AuthService` classes |
| Utility Classes | `Validator`, `Formatter`, `Logger` classes |

## Common Mistakes

```javascript
// ❌ Wrong: Using class without new
class Dog {
  constructor(name) {
    this.name = name;
  }
}

// const buddy = Dog('Buddy'); // TypeError

// ✅ Correct: Always use new
const buddy = new Dog('Buddy');

// ❌ Wrong: Redeclaring class throws error
class Cat {}
// class Cat {} // SyntaxError

// ✅ Correct: Use different names or variables
const AnimalClass = class {};
const BirdClass = class {};
```

## Related Topics

- [[Implement-Chaining]] - Method chaining in classes
- [[What-is-Polymorphism]] - Polymorphism concepts
- [[Override-Methods]] - Overriding class methods
- [[What-is-Encapsulation]] - Encapsulation principles
- [[Implement-Encapsulation]] - Implementing encapsulation
- [[Default-Export]] - Exporting classes as default exports

## Quick Revision

| Concept | Syntax |
|---------|--------|
| Class Declaration | `class MyClass { }` |
| Class Expression | `const MyClass = class { }` |
| Constructor | `constructor() { }` |
| Method | `methodName() { }` |
| Static Method | `static methodName() { }` |
| Instance | `const obj = new MyClass()` |
| Getter | `get propertyName() { }` |
| Setter | `set propertyName(value) { }` |
