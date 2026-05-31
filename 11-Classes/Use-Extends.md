# How to Use `extends`

The `extends` keyword creates a class that inherits from another class, enabling code reuse and establishing an "is-a" relationship between classes.

## Definition

`extends` allows a child class (subclass) to inherit properties and methods from a parent class (superclass). This is JavaScript's implementation of class-based inheritance.

## Syntax

```javascript
class Parent {
  constructor() {
    // parent constructor
  }

  parentMethod() {
    // parent method
  }
}

class Child extends Parent {
  constructor() {
    super(); // Must call super() before using `this`
    // child constructor
  }

  childMethod() {
    // child method
  }
}
```

## Basic Example

```javascript
class Animal {
  constructor(name, sound) {
    this.name = name;
    this.sound = sound;
  }

  speak() {
    return `${this.name} says ${this.sound}`;
  }
}

class Dog extends Animal {
  constructor(name) {
    super(name, "Woof"); // Call parent constructor
  }

  fetch() {
    return `${this.name} fetches the ball`;
  }
}

const dog = new Dog("Rex");
console.log(dog.speak()); // Output: Rex says Woof
console.log(dog.fetch()); // Output: Rex fetches the ball
```

## Inheriting Methods

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
}

const circle = new Circle("red", 5);
console.log(circle.describe()); // Output: A red shape
console.log(circle.area().toFixed(2)); // Output: 78.54
```

## Overriding Parent Methods

```javascript
class Vehicle {
  constructor(type) {
    this.type = type;
  }

  getInfo() {
    return `Vehicle: ${this.type}`;
  }
}

class ElectricCar extends Vehicle {
  constructor(brand, range) {
    super("Electric");
    this.brand = brand;
    this.range = range;
  }

  getInfo() {
    return `${this.brand} - Range: ${this.range} miles`;
  }
}

const tesla = new ElectricCar("Tesla", 350);
console.log(tesla.getInfo()); // Output: Tesla - Range: 350 miles
```

## Multi-Level Inheritance

```javascript
class LivingThing {
  constructor(name) {
    this.name = name;
  }

  breathe() {
    return `${this.name} is breathing`;
  }
}

class Animal extends LivingThing {
  move() {
    return `${this.name} is moving`;
  }
}

class Bird extends Animal {
  fly() {
    return `${this.name} is flying`;
  }
}

const eagle = new Bird("Eagle");
console.log(eagle.breathe()); // Output: Eagle is breathing
console.log(eagle.move());    // Output: Eagle is moving
console.log(eagle.fly());     // Output: Eagle is flying
```

## Inheritance with Static Methods

```javascript
class MathUtils {
  static square(x) {
    return x * x;
  }

  static cube(x) {
    return x * x * x;
  }
}

class AdvancedMath extends MathUtils {
  static power(base, exponent) {
    return Math.pow(base, exponent);
  }
}

console.log(AdvancedMath.square(4));  // Output: 16
console.log(AdvancedMath.power(2, 8)); // Output: 256
```

## Common Use Cases

- Creating UI component hierarchies (Button extends Component)
- Building error types (ValidationError extends Error)
- Creating specialized collection classes
- Implementing design patterns like Template Method
- Extending framework base classes

## Common Mistakes

1. **Forgetting to call `super()` before using `this`**
   ```javascript
   // Wrong
   class Child extends Parent {
     constructor() {
       this.name = "child"; // ReferenceError!
     }
   }

   // Correct
   class Child extends Parent {
     constructor() {
       super(); // Must be called first
       this.name = "child";
     }
   }
   ```

2. **Extending non-constructable built-ins without proper setup**
   ```javascript
   // Wrong
   class MyArray extends Array {}

   // Correct
   class MyArray extends Array {
     static [Symbol.species] = Array;
   }
   ```

3. **Using `extends` with arrow functions** — Only classes can extend other classes

4. **Assuming `extends` creates a copy** — It creates a live prototype link

## Related Topics

- [[What-is-Constructor]]
- [[What-is-Super]]
- [[Call-Parent]]
- [[What-is-Static]]
- [[What-is-Private]]

## Quick Revision

| Concept | Description |
|---------|-------------|
| Purpose | Create subclass inheriting from parent |
| Syntax | `class Child extends Parent` |
| Requirement | Must call `super()` before `this` |
| Method override | Redefine parent methods in child |
| Multi-level | Child can extend Child |
| Static methods | Inherited automatically |
| `super.method()` | Call parent version of overridden method |
