# What is Encapsulation in JavaScript

## Definition

Encapsulation is an object-oriented programming principle that bundles data (properties) and methods (functions) that operate on that data into a single unit (class), while restricting direct access to some components. It hides internal implementation details and only exposes what is necessary through a public interface.

## Key Concepts

| Concept | Description |
|---------|-------------|
| Data Hiding | Protecting internal state from external modification |
| Public Interface | Methods exposed for interaction |
| Private Members | Internal properties/methods not accessible externally |
| Information Hiding | Keeping implementation details secret |

## Code Examples

### Basic Encapsulation

```javascript
class BankAccount {
  #balance; // Private field
  #pin; // Private field

  constructor(owner, initialBalance, pin) {
    this.owner = owner;
    this.#balance = initialBalance;
    this.#pin = pin;
  }

  // Public method - controlled access
  deposit(amount) {
    if (amount <= 0) {
      throw new Error('Deposit amount must be positive');
    }
    this.#balance += amount;
    return this.#balance;
  }

  withdraw(amount, pin) {
    if (pin !== this.#pin) {
      throw new Error('Invalid PIN');
    }
    if (amount <= 0) {
      throw new Error('Withdrawal amount must be positive');
    }
    if (amount > this.#balance) {
      throw new Error('Insufficient funds');
    }
    this.#balance -= amount;
    return this.#balance;
  }

  // Read-only access
  get balance() {
    return this.#balance;
  }

  // Private method
  #validateAmount(amount) {
    return typeof amount === 'number' && amount > 0;
  }
}

const account = new BankAccount('John', 1000, '1234');
console.log(account.balance); // 1000
account.deposit(500);
console.log(account.balance); // 1500
// account.#balance; // SyntaxError: Private field
```

### Encapsulation with Closure

```javascript
function createCounter(initialValue = 0) {
  let count = initialValue; // Private via closure

  return {
    increment() {
      count++;
      return this;
    },
    decrement() {
      count--;
      return this;
    },
    reset() {
      count = initialValue;
      return this;
    },
    getCount() {
      return count;
    }
  };
}

const counter = createCounter(10);
console.log(counter.getCount()); // 10
counter.increment().increment();
console.log(counter.getCount()); // 12
// counter.count; // undefined - hidden
```

### Encapsulation with WeakMap

```javascript
const _private = new WeakMap();

class SecretVault {
  constructor(masterKey) {
    _private.set(this, {
      secrets: [],
      masterKey: masterKey
    });
  }

  addSecret(secret, key) {
    if (key !== _private.get(this).masterKey) {
      throw new Error('Invalid master key');
    }
    _private.get(this).secrets.push(secret);
    return this;
  }

  getSecrets(key) {
    if (key !== _private.get(this).masterKey) {
      throw new Error('Invalid master key');
    }
    return [..._private.get(this).secrets];
  }

  getSecretCount(key) {
    if (key !== _private.get(this).masterKey) {
      throw new Error('Invalid master key');
    }
    return _private.get(this).secrets.length;
  }
}

const vault = new SecretVault('myKey');
vault.addSecret('password123', 'myKey');
vault.addSecret('api_key_abc', 'myKey');

console.log(vault.getSecretCount('myKey')); // 2
console.log(vault.getSecrets('myKey')); // ['password123', 'api_key_abc']
```

### Encapsulation with Getters/Setters

```javascript
class User {
  #password;
  #email;
  #loginAttempts = 0;

  constructor(username, email, password) {
    this.username = username;
    this.#email = email;
    this.#password = this.#hashPassword(password);
  }

  #hashPassword(password) {
    // Simple hash simulation
    return password.split('').reverse().join('');
  }

  get email() {
    return this.#maskEmail(this.#email);
  }

  set email(newEmail) {
    if (!this.#isValidEmail(newEmail)) {
      throw new Error('Invalid email format');
    }
    this.#email = newEmail;
  }

  #maskEmail(email) {
    const [local, domain] = email.split('@');
    return `${local[0]}***@${domain}`;
  }

  #isValidEmail(email) {
    return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
  }

  login(password) {
    this.#loginAttempts++;
    if (this.#loginAttempts > 3) {
      throw new Error('Account locked');
    }
    return password.split('').reverse().join('') === this.#password;
  }

  get loginAttempts() {
    return this.#loginAttempts;
  }
}

const user = new User('john', 'john@example.com', 'secret123');
console.log(user.email); // "j***@example.com"
console.log(user.loginAttempts); // 0
user.login('wrong'); // false
console.log(user.loginAttempts); // 1
```

## Common Use Cases

| Use Case | Benefit |
|----------|---------|
| Financial Systems | Protect sensitive data like balances, transactions |
| User Management | Secure passwords, personal information |
| Configuration | Hide internal settings, expose controlled access |
| API Clients | Protect API keys, tokens |
| Game State | Prevent cheating by hiding game state |
| Validation | Control how data is modified |

## Common Mistakes

```javascript
// ❌ Wrong: Exposing internal state directly
class BadPassword {
  constructor(password) {
    this.password = password; // Public - can be modified!
  }
}

const bad = new BadPassword('secret');
bad.password = 'hacked'; // No protection!

// ✅ Correct: Use private fields
class GoodPassword {
  #password;
  constructor(password) {
    this.#password = password;
  }

  verify(input) {
    return input === this.#password;
  }
}

// ❌ Wrong: No validation in setters
class BadUser {
  constructor(email) {
    this._email = email;
  }

  set email(value) {
    this._email = value; // No validation
  }
}

// ✅ Correct: Validate in setters
class GoodUser {
  #email;
  constructor(email) {
    this.email = email; // Uses setter with validation
  }

  set email(value) {
    if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(value)) {
      throw new Error('Invalid email');
    }
    this.#email = value;
  }

  get email() {
    return this.#email;
  }
}
```

## Related Topics

- [[Implement-Encapsulation]] - How to implement encapsulation
- [[Create-Class]] - Creating classes in JavaScript
- [[What-is-Polymorphism]] - Polymorphism concepts
- [[Override-Methods]] - Overriding methods
- [[Implement-Chaining]] - Method chaining patterns

## Quick Revision

| Concept | Description |
|---------|-------------|
| Encapsulation | Bundling data + methods, hiding internals |
| Private Fields | `#fieldName` syntax (ES2022) |
| Closure | Private variables via function scope |
| WeakMap | Private storage linked to objects |
| Getters/Setters | Controlled access to properties |
| Data Validation | Validate before modifying state |
| Information Hiding | Only expose necessary interface |
