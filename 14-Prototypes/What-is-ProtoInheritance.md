# What is Prototypal Inheritance

## Definition

**Prototypal inheritance** is JavaScript's mechanism where objects can inherit properties and methods directly from other objects through the prototype chain.

## Basic Inheritance

```javascript
const animal = {
    type: "Animal",
    eat() {
        return `${this.name} is eating`;
    },
    sleep() {
        return `${this.name} is sleeping`;
    }
};

const dog = Object.create(animal);
dog.name = "Rex";
dog.bark = function() {
    return `${this.name} barks`;
};

console.log(dog.eat());   // "Rex is eating" (inherited)
console.log(dog.sleep()); // "Rex is sleeping" (inherited)
console.log(dog.bark());  // "Rex barks" (own)
console.log(dog.type);    // "Animal" (inherited)
```

## Constructor Function Inheritance

```javascript
function Animal(name) {
    this.name = name;
}

Animal.prototype.speak = function() {
    return `${this.name} makes a sound`;
};

function Dog(name, breed) {
    Animal.call(this, name); // call parent constructor
    this.breed = breed;
}

// Set up prototype chain
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;

Dog.prototype.bark = function() {
    return `${this.name} barks`;
};

const rex = new Dog("Rex", "Labrador");
console.log(rex.speak()); // "Rex makes a sound" (inherited)
console.log(rex.bark());  // "Rex barks" (own)
console.log(rex.breed);   // "Labrador" (own)
```

## ES6 Class Inheritance

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

    describe() {
        return `A ${this.color} circle with radius ${this.radius}`;
    }
}

class Rectangle extends Shape {
    constructor(color, width, height) {
        super(color);
        this.width = width;
        this.height = height;
    }

    area() {
        return this.width * this.height;
    }
}

const circle = new Circle("red", 5);
const rect = new Rectangle("blue", 4, 6);

console.log(circle.describe()); // "A red circle with radius 5"
console.log(circle.area());     // 78.54
console.log(rect.describe());   // "A blue shape"
console.log(rect.area());       // 24
```

## Method Overriding

```javascript
class Vehicle {
    start() {
        return "Vehicle starting";
    }

    stop() {
        return "Vehicle stopping";
    }
}

class ElectricCar extends Vehicle {
    start() {
        return "Electric car silently starting";
    }

    charge() {
        return "Charging battery";
    }
}

const tesla = new ElectricCar();

console.log(tesla.start()); // "Electric car silently starting" (overridden)
console.log(tesla.stop());  // "Vehicle stopping" (inherited)
console.log(tesla.charge()); // "Charging battery" (own)
```

## Multiple Inheritance with Mixins

```javascript
const Serializable = {
    serialize() {
        return JSON.stringify(this);
    }
};

const Validatable = {
    validate() {
        return Object.keys(this).length > 0;
    }
};

class User {
    constructor(name) {
        this.name = name;
    }
}

// Apply mixins
Object.assign(User.prototype, Serializable, Validatable);

const user = new User("John");
console.log(user.serialize());  // '{"name":"John"}'
console.log(user.validate());   // true
```

## Prototype Chain Visualization

```javascript
class A {
    methodA() { return "A"; }
}

class B extends A {
    methodB() { return "B"; }
}

class C extends B {
    methodC() { return "C"; }
}

const obj = new C();

// Chain: obj → C.prototype → B.prototype → A.prototype → Object.prototype → null
console.log(obj.methodC()); // "C"
console.log(obj.methodB()); // "B"
console.log(obj.methodA()); // "A"
console.log(obj.toString()); // "[object Object]" (from Object.prototype)
```

## Common Use Cases

- **Code reuse** — share methods across objects
- **OOP patterns** — classes, inheritance, polymorphism
- **Built-in objects** — Array, Object, Function use prototypes
- **Libraries/frameworks** — extending functionality

## Common Mistakes

```javascript
// ❌ Forgetting to call parent constructor
function Parent(name) {
    this.name = name;
}

function Child(name, age) {
    // Parent.call(this, name); // Missing!
    this.age = age;
}

Child.prototype = Object.create(Parent.prototype);
const child = new Child("John", 10);
console.log(child.name); // undefined

// ✅ Always call parent constructor
function ChildFixed(name, age) {
    Parent.call(this, name);
    this.age = age;
}

// ❌ Modifying prototype after creating instances
function Dog() {}
const rex = new Dog();
Dog.prototype.bark = function() { return "Woof"; }; // too late!

// ✅ Define prototype methods before instantiation
Dog.prototype.bark = function() { return "Woof"; };
const rex2 = new Dog();
console.log(rex2.bark()); // "Woof"
```

## Quick Revision

- Prototypal inheritance — objects inherit from other objects
- `Object.create()` — explicit prototype setting
- `extends` — ES6 class inheritance
- `super()` — calls parent constructor/methods
- Prototype chain: child → parent → ... → Object.prototype → null
- Method overriding replaces parent methods
- Mixins enable multiple inheritance patterns

---

## Related Topics

- [[What-is-Prototype]] — Prototypes overview
- [[What-is-PrototypeChain]] — Prototype chain
- [[Create-With-Prototype]] — Creating objects with prototype
- [[What-is-ObjectCreate]] — Object.create()
- [[What-is-InstanceOf]] — instanceof operator
