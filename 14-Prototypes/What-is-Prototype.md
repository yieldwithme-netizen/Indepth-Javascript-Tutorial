# What is a Prototype?

## Definition

A prototype is an **object from which other objects inherit** properties and methods.

## Prototype Chain

```javascript
const person = { name: "John" };

// person → Object.prototype → null

console.log(person.toString()); // from Object.prototype
```

## Creating with Prototype

```javascript
// Constructor function
function Person(name) {
    this.name = name;
}

Person.prototype.greet = function() {
    return `Hello, ${this.name}!`;
};

const john = new Person("John");
console.log(john.greet()); // "Hello, John!"
```

## Object.create

```javascript
const animal = {
    speak() {
        return `${this.name} makes a noise`;
    }
};

const dog = Object.create(animal);
dog.name = "Rex";
console.log(dog.speak()); // "Rex makes a noise"
```

## hasOwnProperty

```javascript
const obj = { name: "John" };

obj.hasOwnProperty("name");  // true
obj.hasOwnProperty("age");   // false
```

## Quick Revision

- Prototype = parent object for inheritance
- Objects inherit from Object.prototype
- Use `Object.create()` to set prototype
- `hasOwnProperty()` checks own properties
- Prototype chain: obj → parent → ... → null

---

## Related Topics

- [[What-is-Prototype]] - Prototypes overview
- [[What-is-PrototypeChain]] - Prototype chain
- [[Use-Proto]] - __proto__
- [[What-is-ObjectCreate]] - Object.create
- [[What-is-Class]] - Classes (syntactic sugar)
