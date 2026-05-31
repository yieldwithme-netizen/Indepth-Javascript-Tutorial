# What is `Object.create()`

## Definition

`Object.create()` creates a **new object with a specified prototype** and optional property descriptors. It gives explicit control over object inheritance.

## Syntax

```javascript
Object.create(proto, propertiesObject);
```

- `proto` — prototype of the new object
- `propertiesObject` (optional) — property descriptors

## Basic Usage

```javascript
const person = {
    greet() {
        return `Hello, I'm ${this.name}`;
    }
};

const john = Object.create(person);
john.name = "John";

console.log(john.greet()); // "Hello, I'm John"
```

## Creating with Property Descriptors

```javascript
const car = Object.create({}, {
    make: {
        value: "Toyota",
        writable: true,
        enumerable: true,
        configurable: true
    },
    model: {
        value: "Camry",
        writable: false,
        enumerable: true,
        configurable: false
    },
    start: {
        value() {
            return `${this.make} ${this.model} started`;
        },
        writable: false,
        enumerable: false
    }
});

console.log(car.make);  // "Toyota"
console.log(car.start()); // "Toyota Camry started"

// car.model = "Honda"; // TypeError: Cannot assign (writable: false)
```

## Inheritance Chain

```javascript
const animal = {
    type: "Animal",
    speak() {
        return `${this.name} makes a sound`;
    }
};

const dog = Object.create(animal);
dog.name = "Rex";

const puppy = Object.create(dog);
puppy.age = 2;

// Chain: puppy → dog → animal → Object.prototype → null
console.log(puppy.speak());   // "Rex makes a sound"
console.log(puppy.type);      // "Animal"
console.log(puppy.age);       // 2
```

## vs `new` Operator

```javascript
function Person(name) {
    this.name = name;
}
Person.prototype.greet = function() {
    return `Hi, ${this.name}`;
};

// Using new
const alice = new Person("Alice");

// Using Object.create
const bob = Object.create(Person.prototype);
Person.call(bob, "Bob"); // manually invoke constructor

console.log(alice.greet()); // "Hi, Alice"
console.log(bob.greet());   // "Hi, Bob"
```

## Creating Objects Without Prototype

```javascript
const bare = Object.create(null);
bare.key = "value";

console.log(bare.toString);    // undefined (no inherited methods)
console.log(bare.__proto__);   // null

// Safe for dictionary-like objects
const dict = Object.create(null);
dict["hasOwn"] = true;
console.log(dict.hasOwnProperty); // undefined (no collision)
```

## Common Use Cases

- **Explicit inheritance** — setting up prototype chain
- **Pure dictionaries** — `Object.create(null)` avoids conflicts
- **Cloning objects** — shallow copy with same prototype
- **Mixins** — combining multiple prototypes

## Common Mistakes

```javascript
// ❌ Forgetting to return from constructor with Object.create
function Animal(name) {
    this.name = name;
}

const cat = Object.create(Animal.prototype);
// Forgot: Animal.call(cat, "Whiskers")
console.log(cat.name); // undefined

// ✅ Always invoke constructor function
const cat2 = Object.create(Animal.prototype);
Animal.call(cat2, "Whiskers");
console.log(cat2.name); // "Whiskers"
```

## Quick Revision

- `Object.create(proto)` — creates object with given prototype
- Second argument — property descriptors (enumerable, writable, configurable)
- `Object.create(null)` — no prototype, safe for dictionaries
- More explicit than `new` operator
- Useful for inheritance and mixins

---

## Related Topics

- [[What-is-Prototype]] — Prototypes overview
- [[What-is-PrototypeChain]] — Prototype chain
- [[Use-Proto]] — __proto__ accessor
- [[Create-With-Prototype]] — Creating objects with prototype
- [[What-is-ProtoInheritance]] — Prototypal inheritance
