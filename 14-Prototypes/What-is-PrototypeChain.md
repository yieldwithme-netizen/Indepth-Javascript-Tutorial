# What is Prototype Chain?

## Definition

The **prototype chain** is a mechanism where objects inherit properties and methods from other objects through an internal `[[Prototype]]` link. Each object has a reference to its prototype, forming a chain that ends at `null`.

## How the Chain Works

```javascript
const obj = { name: "John" };

// Chain: obj → Object.prototype → null

console.log(obj.__proto__);          // Object.prototype
console.log(obj.__proto__.__proto__);// null
```

```
obj
  ↓
Object.prototype
  ↓
null (end of chain)
```

## Accessing Properties

```javascript
const animal = {
    type: "Animal",
    eat() {
        return `${this.name} is eating`;
    }
};

const dog = Object.create(animal);
dog.name = "Rex";

// Property lookup traverses the chain
console.log(dog.eat());        // "Rex is eating" (from animal)
console.log(dog.type);         // "Animal" (from animal)
console.log(dog.toString());   // "[object Object]" (from Object.prototype)
```

## Constructor Function Chain

```javascript
function Person(name) {
    this.name = name;
}

Person.prototype.greet = function() {
    return `Hello, I'm ${this.name}`;
};

function Student(name, grade) {
    Person.call(this, name);
    this.grade = grade;
}

Student.prototype = Object.create(Person.prototype);
Student.prototype.constructor = Student;

Student.prototype.study = function() {
    return `${this.name} is studying`;
};

const alice = new Student("Alice", "A");

// Chain: alice → Student.prototype → Person.prototype → Object.prototype → null
console.log(alice.study());  // "Alice is studying"
console.log(alice.greet());  // "Hello, I'm Alice"
console.log(alice.toString()); // "[object Object]"
```

## Visualizing the Chain

```javascript
function Animal(name) {
    this.name = name;
}

Animal.prototype.speak = function() {
    return `${this.name} makes a sound`;
};

function Cat(name) {
    Animal.call(this, name);
}

Cat.prototype = Object.create(Animal.prototype);
Cat.prototype.constructor = Cat;

Cat.prototype.purr = function() {
    return "Purr";
};

const kitty = new Cat("Whiskers");

// Chain visualization
console.log(Object.getPrototypeOf(kitty) === Cat.prototype);           // true
console.log(Object.getPrototypeOf(Cat.prototype) === Animal.prototype); // true
console.log(Object.getPrototypeOf(Animal.prototype) === Object.prototype); // true
console.log(Object.getPrototypeOf(Object.prototype) === null);         // true
```

## Property Shadowing

```javascript
const parent = {
    color: "red",
    greet() {
        return "Hello from parent";
    }
};

const child = Object.create(parent);
child.color = "blue"; // shadows parent's color

console.log(child.color);  // "blue" (own property)
console.log(child.greet()); // "Hello from parent" (inherited)

delete child.color;
console.log(child.color);  // "red" (inherited)
```

## Common Use Cases

- **Object inheritance** — sharing methods across objects
- **Built-in methods** — `toString()`, `valueOf()` come from prototype
- **Constructor patterns** — `new` sets up the chain automatically
- **`Object.create()`** — explicitly setting prototype

## Common Mistakes

```javascript
// ❌ Breaking the chain by overwriting prototype incorrectly
function Dog(name) { this.name = name; }
Dog.prototype = { bark() { return "Woof!"; } };

const rex = new Dog("Rex");
console.log(rex.constructor === Dog); // false!

// ✅ Preserve constructor
function Cat(name) { this.name = name; }
Cat.prototype = Object.create(Cat.prototype);
Cat.prototype.constructor = Cat;
```

## Quick Revision

- Prototype chain = object → prototype → ... → null
- Property lookup traverses the chain upward
- `Object.getPrototypeOf()` retrieves prototype safely
- `new` automatically sets up the chain
- `Object.create()` lets you set prototype explicitly
- Chain ends at `null` (after `Object.prototype`)

---

## Related Topics

- [[What-is-Prototype]] — Prototypes overview
- [[Use-Proto]] — __proto__ accessor
- [[What-is-ObjectCreate]] — Object.create()
- [[Create-With-Prototype]] — Creating objects with prototype
- [[What-is-ProtoInheritance]] — Prototypal inheritance
