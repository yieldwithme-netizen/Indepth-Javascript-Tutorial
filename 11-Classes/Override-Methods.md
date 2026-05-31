# How to Override Methods in JavaScript Classes

## Definition

Method overriding is a feature in OOP that allows a subclass to provide a specific implementation of a method that is already defined in its parent class. The overridden method in the child class replaces or extends the behavior of the parent class method.

## Syntax

```javascript
class Parent {
  methodName() {
    // Parent implementation
  }
}

class Child extends Parent {
  methodName() {
    // Child implementation (overrides parent)
    super.methodName(); // Optional: call parent's version
  }
}
```

## Code Examples

### Basic Method Overriding

```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }

  speak() {
    return `${this.name} makes a sound`;
  }

  move() {
    return `${this.name} moves`;
  }
}

class Dog extends Animal {
  speak() {
    return `${this.name} barks`;
  }

  move() {
    return `${this.name} runs on four legs`;
  }
}

class Cat extends Animal {
  speak() {
    return `${this.name} meows`;
  }

  move() {
    return `${this.name} sneaks quietly`;
  }
}

const dog = new Dog('Rex');
const cat = new Cat('Whiskers');

console.log(dog.speak()); // "Rex barks"
console.log(cat.speak()); // "Whiskers meows"
console.log(dog.move()); // "Rex runs on four legs"
console.log(cat.move()); // "Whiskers sneaks quietly"
```

### Calling Parent Method with super

```javascript
class Vehicle {
  constructor(brand) {
    this.brand = brand;
  }

  start() {
    return `${this.brand} engine started`;
  }

  getInfo() {
    return `Brand: ${this.brand}`;
  }
}

class ElectricCar extends Vehicle {
  constructor(brand, batteryLevel) {
    super(brand);
    this.batteryLevel = batteryLevel;
  }

  start() {
    const parentResult = super.start();
    return `${parentResult} (Electric mode, battery: ${this.batteryLevel}%)`;
  }

  getInfo() {
    const parentInfo = super.getInfo();
    return `${parentInfo}, Battery: ${this.batteryLevel}%`;
  }
}

const tesla = new ElectricCar('Tesla', 85);
console.log(tesla.start());
// "Tesla engine started (Electric mode, battery: 85%)"
console.log(tesla.getInfo());
// "Brand: Tesla, Battery: 85%"
```

### Overriding toString and valueOf

```javascript
class Money {
  constructor(amount, currency = 'USD') {
    this.amount = amount;
    this.currency = currency;
  }

  toString() {
    return `${this.currency} ${this.amount.toFixed(2)}`;
  }

  valueOf() {
    return this.amount;
  }

  toJSON() {
    return {
      amount: this.amount,
      currency: this.currency
    };
  }
}

class DiscountedMoney extends Money {
  constructor(amount, currency, discountPercent) {
    super(amount, currency);
    this.discountPercent = discountPercent;
  }

  get discountedAmount() {
    return this.amount * (1 - this.discountPercent / 100);
  }

  toString() {
    return `${super.toString()} (Discounted: ${this.discountedAmount.toFixed(2)})`;
  }

  valueOf() {
    return this.discountedAmount;
  }
}

const price = new Money(100);
const salePrice = new DiscountedMoney(100, 'USD', 20);

console.log(price.toString()); // "USD 100.00"
console.log(salePrice.toString()); // "USD 100.00 (Discounted: 80.00)"
console.log(salePrice.valueOf()); // 80
```

### Overriding Error Classes

```javascript
class AppError extends Error {
  constructor(message, code, statusCode = 500) {
    super(message);
    this.name = 'AppError';
    this.code = code;
    this.statusCode = statusCode;
  }

  toJSON() {
    return {
      name: this.name,
      message: this.message,
      code: this.code,
      statusCode: this.statusCode
    };
  }
}

class ValidationError extends AppError {
  constructor(message, field) {
    super(message, 'VALIDATION_ERROR', 400);
    this.name = 'ValidationError';
    this.field = field;
  }

  toJSON() {
    return {
      ...super.toJSON(),
      field: this.field
    };
  }
}

class NotFoundError extends AppError {
  constructor(resource) {
    super(`${resource} not found`, 'NOT_FOUND', 404);
    this.name = 'NotFoundError';
    this.resource = resource;
  }
}

const validationErr = new ValidationError('Invalid email', 'email');
const notFoundErr = new NotFoundError('User');

console.log(validationErr.toJSON());
// { name: 'ValidationError', message: 'Invalid email', code: 'VALIDATION_ERROR', statusCode: 400, field: 'email' }

console.log(notFoundErr.toJSON());
// { name: 'NotFoundError', message: 'User not found', code: 'NOT_FOUND', statusCode: 404 }
```

## Common Use Cases

| Use Case | Description |
|----------|-------------|
| Custom Error Types | Override Error class for specific error types |
| Event Emitters | Extend EventEmitter with custom events |
| Data Models | Override base model methods for specific types |
| API Clients | Override request methods for specific endpoints |
| UI Components | Override render methods in component hierarchy |

## Common Mistakes

```javascript
// ❌ Wrong: Not calling super when needed
class Parent {
  constructor(name) {
    this.name = name;
    this.created = Date.now();
  }
}

class Child extends Parent {
  constructor(name, age) {
    // Missing super(name);
    this.age = age; // ReferenceError: Must call super first
  }
}

// ✅ Correct: Call super first
class Child2 extends Parent {
  constructor(name, age) {
    super(name);
    this.age = age;
  }
}

// ❌ Wrong: Overriding without considering parent behavior
class Base {
  validate(data) {
    if (!data) throw new Error('Data required');
    return true;
  }
}

class Extended extends Base {
  validate(data) {
    // Skips parent validation
    return data.length > 0;
  }
}

// ✅ Correct: Include parent validation
class Extended2 extends Base {
  validate(data) {
    super.validate(data); // Call parent validation first
    return data.length > 0;
  }
}
```

## Related Topics

- [[Create-Class]] - Creating classes in JavaScript
- [[What-is-Polymorphism]] - Polymorphism concepts
- [[What-is-Encapsulation]] - Encapsulation principles
- [[Implement-Encapsulation]] - Implementing encapsulation
- [[Implement-Chaining]] - Method chaining patterns

## Quick Revision

| Concept | Key Point |
|---------|-----------|
| Method Overriding | Subclass redefines parent method |
| super.method() | Call parent's version of overridden method |
| super() in constructor | Required before using `this` in subclass |
| Method Signature | Keep same parameters as parent |
| Return Type | Should be compatible with parent's return |
| Override toString | Customize string representation |
| Override valueOf | Customize numeric/primitive value |
