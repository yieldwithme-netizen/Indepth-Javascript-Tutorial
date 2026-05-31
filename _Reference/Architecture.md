# JavaScript Architecture

JavaScript architecture refers to the patterns, principles, and structures used to organize code in scalable, maintainable applications. It encompasses module patterns, design patterns, and application frameworks.

## Definition

JavaScript architecture defines how code is structured, how components interact, and how data flows through an application. Good architecture leads to testable, maintainable, and scalable codebases.

## Module Patterns

```javascript
// Module Pattern (IIFE)
const Counter = (function() {
    let count = 0;
    
    function increment() {
        count++;
    }
    
    function getCount() {
        return count;
    }
    
    return { increment, getCount };
})();

// ES6 Modules
// math.js
export const add = (a, b) => a + b;
export const subtract = (a, b) => a - b;

// app.js
import { add, subtract } from './math.js';
console.log(add(2, 3)); // 5

// Revealing Module Pattern
const BankAccount = (function() {
    let balance = 0;
    
    function deposit(amount) {
        balance += amount;
        return balance;
    }
    
    function withdraw(amount) {
        if (amount > balance) throw new Error('Insufficient funds');
        balance -= amount;
        return balance;
    }
    
    return { deposit, withdraw, balance: () => balance };
})();
```

## MVC Pattern

```javascript
// Model
class UserModel {
    constructor() {
        this.users = [];
    }
    
    addUser(user) {
        this.users.push(user);
    }
    
    getUsers() {
        return [...this.users];
    }
}

// View
class UserView {
    render(users) {
        return users.map(user => `<div>${user.name}</div>`).join('');
    }
}

// Controller
class UserController {
    constructor(model, view) {
        this.model = model;
        this.view = view;
    }
    
    addUser(name) {
        this.model.addUser({ name });
        this.render();
    }
    
    render() {
        const users = this.model.getUsers();
        document.body.innerHTML = this.view.render(users);
    }
}
```

## Observer Pattern

```javascript
class EventEmitter {
    constructor() {
        this.events = {};
    }
    
    on(event, callback) {
        if (!this.events[event]) {
            this.events[event] = [];
        }
        this.events[event].push(callback);
    }
    
    emit(event, ...args) {
        if (this.events[event]) {
            this.events[event].forEach(cb => cb(...args));
        }
    }
    
    off(event, callback) {
        if (this.events[event]) {
            this.events[event] = this.events[event].filter(cb => cb !== callback);
        }
    }
}

// Usage
const emitter = new EventEmitter();
emitter.on('userCreated', (user) => console.log(user));
emitter.emit('userCreated', { name: 'John' });
```

## Component-Based Architecture

```javascript
// React-like component structure
class Component {
    constructor(props) {
        this.props = props;
        this.state = {};
    }
    
    setState(newState) {
        this.state = { ...this.state, ...newState };
        this.render();
    }
    
    render() {
        // Override in subclass
    }
}

class Button extends Component {
    render() {
        return `<button>${this.props.label}</button>`;
    }
}
```

## Common Use Cases

- Single Page Applications (SPAs)
- Server-side rendering (SSR)
- Micro-frontends
- Progressive Web Apps (PWAs)
- Desktop applications with Electron

## Common Mistakes

1. **Tight coupling** - Components depend too heavily on each other
2. **God objects** - Single class/module handles too many responsibilities
3. **Ignoring separation of concerns** - Mixing UI, data, and logic
4. **Over-engineering** - Adding complexity before it's needed
5. **No clear data flow** - Unclear how data moves through the app

## Related Topics

- [[Design Patterns]]
- [[Module System]]
- [[State Management]]
- [[Dependency Injection]]
- [[SOLID Principles]]

## Quick Revision

| Pattern | Purpose |
|---------|---------|
| Module | Encapsulate and expose API |
| MVC | Separate concerns (Model-View-Controller) |
| Observer | Event-driven communication |
| Singleton | Single instance globally |
| Factory | Create objects without specifying class |
| Component | Reusable UI building blocks |
