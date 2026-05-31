# Getters and Setters

## Definition
Getters and setters are special object methods that allow you to define object properties with custom behavior when getting or setting their values. They provide controlled access to object properties.

## Basic Syntax

### Object Literal
```javascript
const person = {
  firstName: 'John',
  lastName: 'Doe',
  
  get fullName() {
    return `${this.firstName} ${this.lastName}`;
  },
  
  set fullName(name) {
    const [first, last] = name.split(' ');
    this.firstName = first;
    this.lastName = last;
  }
};

console.log(person.fullName); // 'John Doe' (getter)
person.fullName = 'Jane Smith'; // setter
console.log(person.firstName); // 'Jane'
console.log(person.lastName); // 'Smith'
```

### Class Syntax
```javascript
class Temperature {
  constructor(celsius) {
    this._celsius = celsius;
  }
  
  get fahrenheit() {
    return (this._celsius * 9/5) + 32;
  }
  
  set fahrenheit(f) {
    this._celsius = (f - 32) * 5/9;
  }
  
  get celsius() {
    return this._celsius;
  }
  
  set celsius(c) {
    this._celsius = c;
  }
}

const temp = new Temperature(100);
console.log(temp.fahrenheit); // 212
temp.fahrenheit = 32;
console.log(temp.celsius); // 0
```

## Practical Examples

### Validation with Setters
```javascript
class User {
  constructor(name, age) {
    this._name = name;
    this._age = age;
  }
  
  get name() {
    return this._name;
  }
  
  set name(value) {
    if (value.length < 2) {
      throw new Error('Name must be at least 2 characters');
    }
    this._name = value;
  }
  
  get age() {
    return this._age;
  }
  
  set age(value) {
    if (value < 0 || value > 150) {
      throw new Error('Invalid age');
    }
    this._age = value;
  }
}

const user = new User('John', 30);
// user.age = -5; // Error: Invalid age
```

### Computed Properties
```javascript
class Circle {
  constructor(radius) {
    this._radius = radius;
  }
  
  get radius() {
    return this._radius;
  }
  
  set radius(value) {
    this._radius = value;
  }
  
  get area() {
    return Math.PI * this._radius ** 2;
  }
  
  get circumference() {
    return 2 * Math.PI * this._radius;
  }
}

const circle = new Circle(5);
console.log(circle.area); // 78.5398...
console.log(circle.circumference); // 31.4159...
```

## Common Use Cases
- Data validation on property assignment
- Computed/derived properties
- Encapsulation and data hiding
- Creating read-only or write-only properties
- Triggering side effects when properties change

## Common Mistakes
- Forgetting to use backing properties (_ prefixed)
- Infinite loops in getters/setters
- Not handling edge cases in setters
- Overusing getters/setters for simple properties
- Confusing getter syntax with method syntax

## Quick Revision Summary
- `get propertyName()` - defines a getter
- `set propertyName(value)` - defines a setter
- Enable controlled property access
- Great for validation and computed properties
- Use underscore convention for backing properties

## Related Topics
- [[Objects]]
- [[Classes]]
- [[Prototypes]]
- [[Encapsulation]]
- [[OOP]]
