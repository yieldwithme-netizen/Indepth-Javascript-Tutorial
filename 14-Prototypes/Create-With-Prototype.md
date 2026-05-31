# How to Create Objects with Prototype

## Definition

There are multiple ways to create objects with prototypes in JavaScript, each providing different levels of control over inheritance.

## Constructor Functions

```javascript
function Person(name, age) {
    this.name = name;
    this.age = age;
}

Person.prototype.greet = function() {
    return `Hello, I'm ${this.name}`;
};

Person.prototype.isAdult = function() {
    return this.age >= 18;
};

const john = new Person("John", 25);
console.log(john.greet());     // "Hello, I'm John"
console.log(john.isAdult());   // true

// Chain: john → Person.prototype → Object.prototype → null
```

## ES6 Classes

```javascript
class Animal {
    constructor(name) {
        this.name = name;
    }

    speak() {
        return `${this.name} makes a sound`;
    }

    eat(food) {
        return `${this.name} eats ${food}`;
    }
}

class Dog extends Animal {
    constructor(name, breed) {
        super(name);
        this.breed = breed;
    }

    speak() {
        return `${this.name} barks`;
    }

    fetch(item) {
        return `${this.name} fetches the ${item}`;
    }
}

const rex = new Dog("Rex", "German Shepherd");
console.log(rex.speak());  // "Rex barks"
console.log(rex.eat("bone")); // "Rex eats bone"
console.log(rex.fetch("ball")); // "Rex fetches the ball"
```

## Object.create()

```javascript
const baseObject = {
    init(name) {
        this.name = name;
        return this; // enable chaining
    },

    toString() {
        return `[${this.constructor.name}: ${this.name}]`;
    }
};

const user = Object.create(baseObject);
user.init("Admin");

console.log(user.toString()); // "[Object: Admin]"
```

## Factory Functions

```javascript
function createCar(make, model) {
    const car = Object.create(carMethods);
    car.make = make;
    car.model = model;
    return car;
}

const carMethods = {
    start() {
        return `${this.make} ${this.model} started`;
    },
    toString() {
        return `${this.make} ${this.model}`;
    }
};

const toyota = createCar("Toyota", "Camry");
console.log(toyota.start()); // "Toyota Camry started"
```

## Prototype Assignment

```javascript
function Vehicle(type) {
    this.type = type;
}

Vehicle.prototype.start = function() {
    return `${this.type} starting`;
};

// Create prototype object
const vehicleProto = {
    start() {
        return `${this.type} starting`;
    }
};

// Assign prototype
function Truck(type) {
    this.type = type;
}
Truck.prototype = Object.create(vehicleProto);
Truck.prototype.constructor = Truck;

const ford = new Truck("Ford F-150");
console.log(ford.start()); // "Ford F-150 starting"
```

## Common Use Cases

- **Constructor functions** — traditional OOP pattern
- **ES6 classes** — modern, cleaner syntax
- **Object.create()** — explicit prototype control
- **Factory functions** — flexible, no `new` needed

## Common Mistakes

```javascript
// ❌ Forgetting `new` with constructor
function Person(name) {
    this.name = name;
}
const bad = Person("John"); // `this` is window, not new object
console.log(window.name);   // "John" (polluted global)

// ✅ Guard against missing `new`
function SafePerson(name) {
    if (!(this instanceof SafePerson)) {
        return new SafePerson(name);
    }
    this.name = name;
}

// ❌ Modifying prototype after instances created
function Dog() {}
const rex = new Dog();
Dog.prototype.bark = function() { return "Woof"; }; // won't affect rex

// ✅ Define methods before creating instances
Dog.prototype.bark = function() { return "Woof"; };
const rex2 = new Dog();
console.log(rex2.bark()); // "Woof"
```

## Quick Revision

- Constructor functions — use `new` to create instances
- ES6 classes — syntactic sugar for constructors
- `Object.create()` — explicit prototype control
- Factory functions — return new object without `new`
- Always define methods on prototype before creating instances

---

## Related Topics

- [[What-is-Prototype]] — Prototypes overview
- [[What-is-PrototypeChain]] — Prototype chain
- [[What-is-ObjectCreate]] — Object.create()
- [[Use-Proto]] — __proto__ accessor
- [[What-is-ProtoInheritance]] — Prototypal inheritance
