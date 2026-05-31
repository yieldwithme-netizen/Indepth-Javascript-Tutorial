# How to Use `__proto__`

## Definition

`__proto__` is a **legacy accessor property** that provides read/write access to an object's prototype. It's deprecated in favor of `Object.getPrototypeOf()` and `Object.setPrototypeOf()`.

## Basic Usage

```javascript
const animal = {
    speak() {
        return `${this.name} makes a noise`;
    }
};

const dog = {};
dog.__proto__ = animal;
dog.name = "Rex";

console.log(dog.speak()); // "Rex makes a noise"
```

## Reading Prototype

```javascript
const person = { name: "John" };
const student = Object.create(person);

// Reading __proto__
console.log(student.__proto__ === person);  // true
console.log(student.__proto__);             // { name: "John" }
```

## Setting Prototype

```javascript
function Vehicle(type) {
    this.type = type;
}

Vehicle.prototype.drive = function() {
    return `Driving ${this.type}`;
};

const car = new Vehicle("Car");
const motorcycle = {};

// Set prototype after creation
motorcycle.__proto__ = Vehicle.prototype;
Object.setPrototypeOf(motorcycle, Vehicle.prototype); // preferred way

console.log(motorcycle.drive()); // "Driving undefined"
```

## vs `Object.getPrototypeOf()`

```javascript
const parent = { greet() { return "Hello"; } };
const child = Object.create(parent);

// ✅ Recommended (read-only access)
console.log(Object.getPrototypeOf(child) === parent); // true

// ❌ Deprecated (read/write access)
console.log(child.__proto__ === parent); // true

// Setting prototype
// ✅ Recommended
Object.setPrototypeOf(child, { newMethod() {} });

// ❌ Deprecated
child.__proto__ = { newMethod() {} };
```

## Accessing Inherited Properties

```javascript
const base = { baseProp: "from base" };
const derived = Object.create(base);
derived.derivedProp = "from derived";

// Access properties via __proto__
console.log(derived.__proto__.baseProp);     // "from base"
console.log(derived.derivedProp);             // "from derived"
console.log(derived.__proto__.__proto__);     // Object.prototype
```

## Common Use Cases

- Legacy codebases using older JavaScript patterns
- Debugging — inspecting prototype chain
- Learning about prototype mechanics

## Common Mistakes

```javascript
// ❌ Modifying Object.prototype (dangerous!)
Object.prototype.pollute = "bad";
const obj = {};
console.log(obj.pollute); // "bad" (affects ALL objects)

// ❌ Using __proto__ as an object literal property
const bad = {
    __proto__: "string" // doesn't work as intended
};

// ✅ Use Object.create() instead
const good = Object.create(null); // clean object, no prototype
```

## Quick Revision

- `__proto__` = legacy read/write accessor for prototype
- Prefer `Object.getPrototypeOf()` (read) and `Object.setPrototypeOf()` (write)
- `__proto__` links object to its parent
- Avoid setting `__proto__` on existing objects
- Don't modify `Object.prototype`

---

## Related Topics

- [[What-is-Prototype]] — Prototypes overview
- [[What-is-PrototypeChain]] — Prototype chain
- [[What-is-ObjectCreate]] — Object.create()
- [[Create-With-Prototype]] — Creating objects with prototype
- [[What-is-ProtoInheritance]] — Prototypal inheritance
