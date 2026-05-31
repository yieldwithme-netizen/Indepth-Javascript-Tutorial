# What is `instanceof`

## Definition

`instanceof` checks if an object's **prototype chain contains** the prototype property of a constructor. It returns `true` or `false`.

## Basic Usage

```javascript
function Person(name) {
    this.name = name;
}

const john = new Person("John");

console.log(john instanceof Person);  // true
console.log(john instanceof Object);  // true (inherited)
console.log(john instanceof Array);   // false
```

## How It Works

```javascript
function Animal() {}
function Dog() {}
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;

const rex = new Dog();

// Chain: rex → Dog.prototype → Animal.prototype → Object.prototype → null
console.log(rex instanceof Dog);     // true
console.log(rex instanceof Animal);  // true
console.log(rex instanceof Object);  // true
```

## With Classes

```javascript
class Vehicle {
    constructor(type) {
        this.type = type;
    }
}

class Car extends Vehicle {
    constructor(make) {
        super("Car");
        this.make = make;
    }
}

const tesla = new Car("Tesla");

console.log(tesla instanceof Car);     // true
console.log(tesla instanceof Vehicle); // true
console.log(tesla instanceof Object);  // true
```

## Checking Multiple Types

```javascript
function checkType(obj) {
    if (obj instanceof Array) {
        return "Array";
    } else if (obj instanceof Date) {
        return "Date";
    } else if (obj instanceof RegExp) {
        return "RegExp";
    } else if (obj instanceof Object) {
        return "Object";
    }
    return "Unknown";
}

console.log(checkType([1, 2, 3]));         // "Array"
console.log(checkType(new Date()));         // "Date"
console.log(checkType(/test/));             // "RegExp"
console.log(checkType({ name: "John" }));  // "Object"
```

## Limitations

```javascript
// ❌ Doesn't work across different frames/contexts
const iframe = document.createElement("iframe");
document.body.appendChild(iframe);
const iframeArray = iframe.contentWindow.Array;
const arr = new iframeArray();

console.log(arr instanceof Array); // false (different Array!)
console.log(Array.isArray(arr));   // true ✅

// ❌ Doesn't work with primitives
console.log("hello" instanceof String); // false
console.log(42 instanceof Number);      // false

// ✅ Use typeof for primitives
console.log(typeof "hello"); // "string"
console.log(typeof 42);      // "number"
```

## instanceof vs typeof vs Object.prototype.toString

```javascript
const obj = { name: "John" };
const arr = [1, 2, 3];
const fn = function() {};
const str = "hello";

// instanceof
console.log(obj instanceof Object); // true
console.log(arr instanceof Array);  // true

// typeof
console.log(typeof obj); // "object"
console.log(typeof arr); // "object" (unhelpful)
console.log(typeof fn);  // "function"

// Object.prototype.toString
console.log(Object.prototype.toString.call(obj)); // "[object Object]"
console.log(Array.isArray(arr));                  // true
```

## Common Use Cases

- **Type checking** — verify object type
- **Polymorphism** — branch based on type
- **Error handling** — check error types
- **Input validation** — ensure correct types

## Common Mistakes

```javascript
// ❌ Using instanceof for primitives
console.log("hello" instanceof String); // false

// ✅ Use typeof
console.log(typeof "hello"); // "string"

// ❌ Assuming instanceof across frames
// (different contexts have different prototypes)

// ✅ Use Array.isArray() for arrays
console.log(Array.isArray([])); // true
```

## Quick Revision

- `instanceof` checks prototype chain
- Returns `true` if constructor's prototype is in chain
- Works with classes and constructor functions
- Doesn't work with primitives
- Doesn't work across different frames/contexts
- Use `typeof` for primitives, `Array.isArray()` for arrays

---

## Related Topics

- [[Use-InstanceOf]] — Using instanceof
- [[What-is-PrototypeChain]] — Prototype chain
- [[What-is-Prototype]] — Prototypes overview
- [[Create-With-Prototype]] — Creating objects with prototype
- [[Check-Properties]] — Checking properties
