# How to Define Methods in Objects

**Methods** are functions stored as object properties. They allow objects to perform actions using their own data.

## Method Definition Syntax

```javascript
const calculator = {
  add: function(a, b) {
    return a + b;
  },
  subtract(a, b) {  // Shorthand method syntax
    return a - b;
  }
};

console.log(calculator.add(5, 3));      // 8
console.log(calculator.subtract(5, 3)); // 2
```

## Using `this` Keyword

```javascript
const person = {
  firstName: "John",
  lastName: "Doe",
  fullName() {
    return `${this.firstName} ${this.lastName}`;
  }
};

console.log(person.fullName()); // "John Doe"
```

## Constructor Functions with Methods

```javascript
function Person(name, age) {
  this.name = name;
  this.age = age;
}

Person.prototype.greet = function() {
  return `Hi, I'm ${this.name}`;
};

const john = new Person("John", 30);
console.log(john.greet()); // "Hi, I'm John"
```

## Class Methods (ES6+)

```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }

  speak() {
    return `${this.name} makes a sound`;
  }

  static create(name) {
    return new Animal(name);
  }
}

const dog = new Animal("Dog");
console.log(dog.speak()); // "Dog makes a sound"

const cat = Animal.create("Cat");
console.log(cat.speak()); // "Cat makes a sound"
```

## Common Use Cases

- Encapsulating behavior with data
- Creating reusable functionality
- Building object-oriented patterns
- Implementing design patterns

## Common Mistakes

```javascript
const obj = {
  value: 10,
  // ❌ Arrow function doesn't bind `this` correctly
  getValueArrow: () => {
    return this.value; // undefined (this refers to outer scope)
  },
  // ✅ Regular function binds `this` to object
  getValue() {
    return this.value; // 10
  }
};

// ❌ Forgetting to call the method
console.log(obj.getValue); // function definition, not result

// ✅ Call the method with parentheses
console.log(obj.getValue()); // 10
```

## Related Topics

- [[Define-Objects]]
- [[Access-Properties]]
- [[This-Keyword]]
- [[Prototypes]]
- [[Classes]]

## Quick Revision

| Method Type | Syntax | Example |
|-------------|--------|---------|
| Function property | `key: function() {}` | `obj.method = function() {}` |
| Shorthand | `method() {}` | `{ greet() {} }` |
| Class method | `methodName() {}` | `class { method() {} }` |
| Static | `static method() {}` | `Class.create()` |

**Key Point:** Use shorthand syntax for cleaner code; remember `this` context in methods.