# Classes

## Definition

**Classes** are syntactic sugar over JavaScript's prototype-based inheritance. Introduced in ES6, they provide a cleaner, more structured way to create objects, implement inheritance, and organize code using object-oriented programming (OOP) principles.

## Class Declaration

```javascript
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  greet() {
    return `Hi, I'm ${this.name} and I'm ${this.age} years old.`;
  }
}

const alice = new Person("Alice", 30);
console.log(alice.greet());
// "Hi, I'm Alice and I'm 30 years old."
```

## Constructor

The `constructor` method is called automatically when a new instance is created:

```javascript
class Car {
  constructor(make, model, year) {
    this.make = make;
    this.model = model;
    this.year = year;
    this.isRunning = false;
  }
}

const car = new Car("Toyota", "Camry", 2024);
```

## Methods

```javascript
class Calculator {
  constructor() {
    this.result = 0;
  }

  add(value) {
    this.result += value;
    return this; // Enable method chaining
  }

  subtract(value) {
    this.result -= value;
    return this;
  }

  getResult() {
    return this.result;
  }
}

const calc = new Calculator();
const result = calc.add(10).subtract(3).getResult();
console.log(result); // 7
```

## Static Methods

Called on the class itself, not on instances:

```javascript
class MathUtils {
  static add(a, b) {
    return a + b;
  }

  static multiply(a, b) {
    return a * b;
  }
}

console.log(MathUtils.add(2, 3)); // 5
// const m = new MathUtils();
// m.add(2, 3); // TypeError: m.add is not a function
```

## Inheritance (`extends`)

```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }

  speak() {
    return `${this.name} makes a sound.`;
  }
}

class Dog extends Animal {
  speak() {
    return `${this.name} barks!`;
  }
}

class Cat extends Animal {
  speak() {
    return `${this.name} meows!`;
  }
}

const dog = new Dog("Rex");
const cat = new Cat("Whiskers");

console.log(dog.speak()); // "Rex barks!"
console.log(cat.speak()); // "Whiskers meows!"
```

## Super Constructor

Use `super()` to call the parent class constructor:

```javascript
class Vehicle {
  constructor(type, wheels) {
    this.type = type;
    this.wheels = wheels;
  }
}

class Truck extends Vehicle {
  constructor(make, model) {
    super("truck", 4); // Call parent constructor
    this.make = make;
    this.model = model;
  }
}

const truck = new Truck("Ford", "F-150");
console.log(truck); // { type: "truck", wheels: 4, make: "Ford", model: "F-150" }
```

## Getters and Setters

```javascript
class BankAccount {
  #balance; // Private field

  constructor(owner, balance) {
    this.owner = owner;
    this.#balance = balance;
  }

  get balance() {
    return this.#balance;
  }

  set balance(amount) {
    if (amount < 0) {
      throw new Error("Balance cannot be negative");
    }
    this.#balance = amount;
  }

  deposit(amount) {
    this.balance += amount;
  }

  withdraw(amount) {
    if (amount > this.#balance) {
      throw new Error("Insufficient funds");
    }
    this.balance -= amount;
  }
}

const account = new BankAccount("Alice", 1000);
account.deposit(500);
console.log(account.balance); // 1500
// account.balance = -100; // Error: Balance cannot be negative
```

## Private Fields

```javascript
class User {
  #password;

  constructor(username, password) {
    this.username = username;
    this.#password = this.#hashPassword(password);
  }

  #hashPassword(password) {
    return password.split("").reverse().join("");
  }

  checkPassword(password) {
    return this.#password === this.#hashPassword(password);
  }
}

const user = new User("alice", "secret123");
console.log(user.checkPassword("secret123")); // true
// console.log(user.#password); // SyntaxError
```

## Common Use Cases

- **Modeling real-world entities** (User, Product, Order)
- **UI components** (Button, Modal, Form)
- **Data structures** (Stack, Queue, LinkedList)
- **Service layers** (API service, Auth service)

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Forgetting `new` keyword | Use PascalCase for class names as reminder |
| Not calling `super()` in child class | Always call `super()` before using `this` |
| Mutating shared reference properties | Initialize references in constructor |
| Using `var` inside classes | Use class fields or assign in constructor |

## Quick Revision

- Classes are syntactic sugar over prototypes
- `constructor` initializes instance properties
- Methods define object behavior
- `static` methods belong to the class, not instances
- `extends` and `super` enable inheritance
- `#` prefix creates private fields
- Getters/setters control property access

## Related Topics

- [[Prototypes]]
- [[Inheritance]]
- [[Constructor-Functions]]
- [[Factory-Functions]]
- [[Modules]]
- [[Encapsulation]]
