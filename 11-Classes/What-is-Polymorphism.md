# What is Polymorphism in JavaScript

## Definition

Polymorphism is an object-oriented programming concept that allows objects of different classes to be treated as objects of a common superclass. The same method can behave differently depending on which object calls it. In JavaScript, polymorphism is achieved through method overriding and duck typing.

## Types of Polymorphism

1. **Compile-time (Static) Polymorphism** - Method overloading
2. **Runtime (Dynamic) Polymorphism** - Method overriding

## Code Examples

### Runtime Polymorphism with Method Overriding

```javascript
class Shape {
  constructor(name) {
    this.name = name;
  }

  area() {
    return 0;
  }

  describe() {
    return `${this.name} has area: ${this.area()}`;
  }
}

class Circle extends Shape {
  constructor(radius) {
    super('Circle');
    this.radius = radius;
  }

  area() {
    return Math.PI * this.radius ** 2;
  }
}

class Rectangle extends Shape {
  constructor(width, height) {
    super('Rectangle');
    this.width = width;
    this.height = height;
  }

  area() {
    return this.width * this.height;
  }
}

class Triangle extends Shape {
  constructor(base, height) {
    super('Triangle');
    this.base = base;
    this.height = height;
  }

  area() {
    return 0.5 * this.base * this.height;
  }
}

const shapes = [
  new Circle(5),
  new Rectangle(4, 6),
  new Triangle(3, 8)
];

shapes.forEach(shape => {
  console.log(shape.describe());
});
// "Circle has area: 78.53981633974483"
// "Rectangle has area: 24"
// "Triangle has area: 12"
```

### Polymorphism with Interfaces

```javascript
class Printable {
  print() {
    throw new Error('print() must be implemented');
  }
}

class Document extends Printable {
  constructor(content) {
    super();
    this.content = content;
  }

  print() {
    console.log(`Document: ${this.content}`);
  }
}

class Image extends Printable {
  constructor(url) {
    super();
    this.url = url;
  }

  print() {
    console.log(`Printing image from: ${this.url}`);
  }
}

function printItem(item) {
  if (item instanceof Printable) {
    item.print();
  }
}

printItem(new Document('My Report'));
printItem(new Image('photo.jpg'));
```

### Duck Typing Polymorphism

```javascript
class Dog {
  speak() {
    return 'Woof!';
  }

  move() {
    return 'Running on four legs';
  }
}

class Bird {
  speak() {
    return 'Tweet!';
  }

  move() {
    return 'Flying with wings';
  }
}

class Fish {
  speak() {
    return 'Blub!';
  }

  move() {
    return 'Swimming with fins';
  }
}

// Duck typing - no common interface needed
function interactWithAnimal(animal) {
  console.log(animal.speak());
  console.log(animal.move());
}

interactWithAnimal(new Dog());
interactWithAnimal(new Bird());
interactWithAnimal(new Fish());
```

### Polymorphism with Arrays

```javascript
class PaymentMethod {
  pay(amount) {
    throw new Error('pay() must be implemented');
  }

  getFee(amount) {
    return 0;
  }
}

class CreditCard extends PaymentMethod {
  pay(amount) {
    return `Paid $${amount} with credit card`;
  }

  getFee(amount) {
    return amount * 0.02;
  }
}

class PayPal extends PaymentMethod {
  pay(amount) {
    return `Paid $${amount} with PayPal`;
  }

  getFee(amount) {
    return amount * 0.03;
  }
}

class BankTransfer extends PaymentMethod {
  pay(amount) {
    return `Paid $${amount} via bank transfer`;
  }

  getFee(amount) {
    return amount < 1000 ? 5 : 0;
  }
}

function processPayment(method, amount) {
  const fee = method.getFee(amount);
  const total = amount + fee;
  console.log(method.pay(amount));
  console.log(`Fee: $${fee}, Total: $${total}`);
}

processPayment(new CreditCard(), 100);
processPayment(new PayPal(), 100);
processPayment(new BankTransfer(), 500);
```

## Common Use Cases

| Use Case | Description |
|----------|-------------|
| Plugin Systems | Different plugins implementing same interface |
| Game Entities | Players, enemies, NPCs with different behaviors |
| Payment Processing | Multiple payment methods with same interface |
| Notification Systems | Email, SMS, push notifications |
| Data Formats | JSON, XML, CSV parsers |
| UI Components | Buttons, inputs, modals with common methods |

## Common Mistakes

```javascript
// ❌ Wrong: Assuming all objects have same methods
class Cat {
  meow() {
    return 'Meow!';
  }
}

class Dog {
  bark() {
    return 'Woof!';
  }
}

function makeSound(animal) {
  return animal.meow(); // Dog doesn't have meow!
}

// ✅ Correct: Use polymorphism properly
class Animal {
  makeSound() {
    throw new Error('makeSound() must be implemented');
  }
}

class Dog2 extends Animal {
  makeSound() {
    return 'Woof!';
  }
}

class Cat2 extends Animal {
  makeSound() {
    return 'Meow!';
  }
}

function makeSound2(animal) {
  return animal.makeSound(); // Works with any Animal subclass
}
```

## Related Topics

- [[Create-Class]] - Creating classes in JavaScript
- [[Override-Methods]] - Overriding methods for polymorphism
- [[What-is-Encapsulation]] - Encapsulation principles
- [[Implement-Encapsulation]] - Implementing encapsulation
- [[Implement-Chaining]] - Method chaining patterns

## Quick Revision

| Concept | Description |
|---------|-------------|
| Polymorphism | Same interface, different implementations |
| Method Overriding | Subclass provides specific implementation |
| Duck Typing | Object's type determined by methods present |
| Runtime Polymorphism | Method behavior determined at runtime |
| Interface | Common contract for related classes |
| instanceof | Check object's class hierarchy |
| super | Call parent class methods |
