# What is SOLID Principle

## Definition

SOLID is an acronym representing five design principles for writing maintainable, scalable, and robust object-oriented code. These principles were introduced by Robert C. Martin (Uncle Bob) and help developers create software that is easy to understand, extend, and maintain over time.

The five principles are:

- **S** - Single Responsibility Principle
- **O** - Open/Closed Principle
- **L** - Liskov Substitution Principle
- **I** - Interface Segregation Principle
- **D** - Dependency Inversion Principle

## 1. Single Responsibility Principle (SRP)

A class or module should have only one reason to change. Each component should be responsible for a single piece of functionality.

```javascript
// BAD: Multiple responsibilities in one class
class UserManager {
  constructor(database) {
    this.database = database;
  }

  createUser(userData) {
    // Creates user
  }

  sendWelcomeEmail(user) {
    // Sends email - different responsibility
  }

  generateReport(user) {
    // Generates report - another responsibility
  }
}

// GOOD: Each class has one responsibility
class UserRepository {
  constructor(database) {
    this.database = database;
  }

  createUser(userData) {
    return this.database.insert('users', userData);
  }
}

class EmailService {
  sendWelcomeEmail(user) {
    // Sends welcome email
  }
}

class ReportGenerator {
  generateUserReport(user) {
    // Generates user report
  }
}
```

## 2. Open/Closed Principle (OCP)

Software entities should be open for extension but closed for modification. You should be able to add new functionality without changing existing code.

```javascript
// BAD: Modifying existing code to add new discount types
class DiscountCalculator {
  calculate(type, price) {
    if (type === 'percentage') {
      return price * 0.9;
    } else if (type === 'fixed') {
      return price - 10;
    }
    // Adding new type requires modifying this method
  }
}

// GOOD: Open for extension, closed for modification
class DiscountStrategy {
  calculate(price) {
    throw new Error('calculate() must be implemented');
  }
}

class PercentageDiscount extends DiscountStrategy {
  calculate(price) {
    return price * 0.9;
  }
}

class FixedDiscount extends DiscountStrategy {
  calculate(price) {
    return price - 10;
  }
}

// New discount type without modifying existing code
class BuyOneGetOneFree extends DiscountStrategy {
  calculate(price) {
    return price / 2;
  }
}

class DiscountCalculator {
  calculate(strategy, price) {
    return strategy.calculate(price);
  }
}
```

## 3. Liskov Substitution Principle (LSP)

Objects of a superclass should be replaceable with objects of its subclasses without breaking the application.

```javascript
// BAD: Subclass breaks parent behavior
class Bird {
  fly() {
    return 'Flying';
  }
}

class Penguin extends Bird {
  fly() {
    throw new Error('Penguins cannot fly'); // Breaks parent contract
  }
}

// GOOD: Proper substitution
class Bird {
  move() {
    return 'Moving';
  }
}

class FlyingBird extends Bird {
  fly() {
    return 'Flying';
  }
}

class SwimmingBird extends Bird {
  swim() {
    return 'Swimming';
  }
}

class Penguin extends SwimmingBird {
  // Penguin can substitute SwimmingBird without breaking behavior
}

class Sparrow extends FlyingBird {
  // Sparrow can substitute FlyingBird without breaking behavior
}
```

## 4. Interface Segregation Principle (ISP)

Clients should not be forced to depend on interfaces they do not use. Keep interfaces small and specific.

```javascript
// BAD: Fat interface forces unnecessary implementation
class Worker {
  work() {}
  eat() {}
  sleep() {}
}

class Robot extends Worker {
  work() {
    return 'Working';
  }
  eat() {
    throw new Error('Robots do not eat'); // Forced to implement
  }
  sleep() {
    throw new Error('Robots do not sleep'); // Forced to implement
  }
}

// GOOD: Segregated interfaces
class Workable {
  work() {
    throw new Error('work() must be implemented');
  }
}

class Feedable {
  eat() {
    throw new Error('eat() must be implemented');
  }
}

class Sleepable {
  sleep() {
    throw new Error('sleep() must be implemented');
  }
}

class HumanWorker extends Workable extends Feedable extends Sleepable {
  work() { return 'Working'; }
  eat() { return 'Eating'; }
  sleep() { return 'Sleeping'; }
}

class RobotWorker extends Workable {
  work() { return 'Working'; }
  // No need to implement eat() or sleep()
}
```

## 5. Dependency Inversion Principle (DIP)

High-level modules should not depend on low-level modules. Both should depend on abstractions. Abstractions should not depend on details.

```javascript
// BAD: High-level module depends on low-level module
class MySQLDatabase {
  query(sql) {
    return `Executing: ${sql}`;
  }
}

class UserService {
  constructor() {
    this.database = new MySQLDatabase(); // Tight coupling
  }

  getUser(id) {
    return this.database.query(`SELECT * FROM users WHERE id = ${id}`);
  }
}

// GOOD: Both depend on abstraction
class DatabaseInterface {
  query(sql) {
    throw new Error('query() must be implemented');
  }
}

class MySQLDatabase extends DatabaseInterface {
  query(sql) {
    return `MySQL: Executing ${sql}`;
  }
}

class MongoDBDatabase extends DatabaseInterface {
  query(sql) {
    return `MongoDB: Executing ${sql}`;
  }
}

class UserService {
  constructor(database) {
    this.database = database; // Depends on abstraction
  }

  getUser(id) {
    return this.database.query(`SELECT * FROM users WHERE id = ${id}`);
  }
}

// Usage - easily swap implementations
const mysql = new MySQLDatabase();
const mongo = new MongoDBDatabase();

const userService1 = new UserService(mysql);
const userService2 = new UserService(mongo);
```

## Common Use Cases

- Designing large-scale applications
- Creating reusable and maintainable code libraries
- Building plugin architectures
- Refactoring legacy code
- Team collaboration on complex projects
- Testing and mocking dependencies

## Common Mistakes

- Applying all principles to every small piece of code (over-engineering)
- Ignoring SOLID principles in small, simple projects
- Creating too many abstractions leading to complexity
- Not considering future requirements when designing
- Mixing responsibilities across modules
- Creating tight coupling between components

## Related Topics

- [[Design-Patterns]]
- [[Refactoring]]
- [[Clean-Code]]
- [[Dependency-Injection]]
- [[Testing]]
- [[Code-Review]]
- [[Architecture]]

## Quick Revision

| Principle | Description |
|-----------|-------------|
| SRP | One class = One responsibility |
| OCP | Open for extension, closed for modification |
| LSP | Subtypes must be substitutable for base types |
| ISP | Many specific interfaces over one general-purpose |
| DIP | Depend on abstractions, not concrete implementations |
