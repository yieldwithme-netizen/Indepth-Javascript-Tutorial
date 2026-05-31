# How to Use `instanceof`

## Definition

`instanceof` is used to check if an object is an instance of a particular constructor or class by examining its prototype chain.

## Basic Examples

```javascript
class Animal {
    constructor(name) {
        this.name = name;
    }
}

class Dog extends Animal {
    bark() {
        return `${this.name} barks`;
    }
}

const rex = new Dog("Rex");

console.log(rex instanceof Dog);     // true
console.log(rex instanceof Animal);  // true
console.log(rex instanceof Object);  // true
```

## Type Checking in Functions

```javascript
function processValue(value) {
    if (value instanceof Array) {
        return value.map(v => v * 2);
    } else if (value instanceof String) {
        return value.toUpperCase();
    } else if (value instanceof Number) {
        return value * 2;
    }
    return value;
}

console.log(processValue([1, 2, 3]));      // [2, 4, 6]
console.log(processValue("hello"));        // "HELLO"
console.log(processValue(5));             // 10
```

## Checking Error Types

```javascript
function handleError(error) {
    if (error instanceof TypeError) {
        console.log("Type Error:", error.message);
    } else if (error instanceof RangeError) {
        console.log("Range Error:", error.message);
    } else if (error instanceof Error) {
        console.log("General Error:", error.message);
    }
}

handleError(new TypeError("Invalid type"));  // "Type Error: Invalid type"
handleError(new RangeError("Out of range")); // "Range Error: Out of range"
```

## Custom Class Validation

```javascript
class Email {
    constructor(value) {
        if (!value.includes("@")) {
            throw new Error("Invalid email");
        }
        this.value = value;
    }
}

class Phone {
    constructor(value) {
        if (!/^\d{10}$/.test(value)) {
            throw new Error("Invalid phone");
        }
        this.value = value;
    }
}

function validateContact(contact) {
    if (contact.email instanceof Email) {
        console.log("Valid email:", contact.email.value);
    }
    if (contact.phone instanceof Phone) {
        console.log("Valid phone:", contact.phone.value);
    }
}

const contact = {
    email: new Email("john@example.com"),
    phone: new Phone("1234567890")
};

validateContact(contact);
```

## Multiple Inheritance Check

```javascript
class Serializable {
    serialize() {
        return JSON.stringify(this);
    }
}

class Validatable {
    validate() {
        return Object.keys(this).length > 0;
    }
}

class User extends Serializable {
    constructor(name) {
        super();
        this.name = name;
    }
}

const user = new User("John");

console.log(user instanceof User);          // true
console.log(user instanceof Serializable);  // true
console.log(user instanceof Object);        // true
console.log(user instanceof Validatable);   // false
```

## Alternative Methods

```javascript
class Shape {
    constructor(type) {
        this.type = type;
    }
}

class Circle extends Shape {
    constructor(radius) {
        super("circle");
        this.radius = radius;
    }
}

const c = new Circle(5);

// instanceof
console.log(c instanceof Circle); // true

// constructor check
console.log(c.constructor === Circle); // true

// typeof (limited)
console.log(typeof c); // "object"

// Object.prototype.toString
console.log(Object.prototype.toString.call(c)); // "[object Object]"

// Array-specific check
console.log(Array.isArray([])); // true
```

## Common Use Cases

- **Input validation** — ensure correct object types
- **Error handling** — catch specific error types
- **Polymorphism** — dispatch based on type
- **API responses** — validate response structure

## Common Mistakes

```javascript
// ❌ Using instanceof with primitives
const str = "hello";
console.log(str instanceof String); // false

// ✅ Use typeof
console.log(typeof str); // "string"

// ❌ Assuming instanceof works across iframes
const iframeArray = iframe.contentWindow.Array;
const arr = new iframeArray();
console.log(arr instanceof Array); // false

// ✅ Use Array.isArray()
console.log(Array.isArray(arr)); // true

// ❌ Not handling null/undefined
const obj = null;
// console.log(obj instanceof Object); // false (no error)

// ✅ Guard first
if (obj && obj instanceof Object) {
    console.log("is object");
}
```

## Quick Revision

- `instanceof` checks if constructor's prototype is in object's chain
- Use for type checking with classes and constructors
- Doesn't work with primitives (use `typeof`)
- Doesn't work across different frames (use `Array.isArray()`)
- Useful for error handling and input validation
- Chain inheritance: `obj instanceof Parent` is `true` for subclasses

---

## Related Topics

- [[What-is-InstanceOf]] — instanceof overview
- [[What-is-PrototypeChain]] — Prototype chain
- [[What-is-Prototype]] — Prototypes overview
- [[Create-With-Prototype]] — Creating objects with prototype
- [[Check-Properties]] — Checking properties
