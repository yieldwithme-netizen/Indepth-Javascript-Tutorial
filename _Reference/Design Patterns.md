# Design Patterns

## Definition

Design patterns are **reusable solutions** to common software design problems.

## Singleton Pattern

```javascript
class Singleton {
    constructor() {
        if (Singleton.instance) {
            return Singleton.instance;
        }
        this.data = {};
        Singleton.instance = this;
    }
    
    getData() {
        return this.data;
    }
    
    setData(data) {
        this.data = data;
    }
}

const s1 = new Singleton();
const s2 = new Singleton();
console.log(s1 === s2); // true
```

## Observer Pattern

```javascript
class Observer {
    constructor() {
        this.observers = [];
    }
    
    subscribe(fn) {
        this.observers.push(fn);
    }
    
    unsubscribe(fn) {
        this.observers = this.observers.filter(obs => obs !== fn);
    }
    
    notify(data) {
        this.observers.forEach(fn => fn(data));
    }
}
```

## Factory Pattern

```javascript
function createUser(type) {
    if (type === 'admin') {
        return { role: 'admin', permissions: ['read', 'write', 'delete'] };
    }
    return { role: 'user', permissions: ['read'] };
}

const admin = createUser('admin');
const user = createUser('user');
```

## Quick Revision

- Singleton: one instance only
- Observer: publish-subscribe
- Factory: create objects without new
- Module: encapsulate code
- Strategy: swap algorithms

---

## Related Topics

- [[Design-Patterns]] - [[Design-Patterns|Design patterns]]
- [[Singleton-Pattern]] - [[Singleton-Pattern|Singleton]]
- [[Module-Pattern]] - [[Module-Pattern|Module pattern]]
- [[Factory-Functions]] - [[Factory-Functions|Factory functions]]
