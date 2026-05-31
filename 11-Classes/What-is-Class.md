# What is a Class?

## Definition

A class is a **blueprint for creating objects** with shared properties and methods.

## Basic Syntax

```javascript
class Person {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
    
    greet() {
        return `Hello, ${this.name}!`;
    }
}

const john = new Person("John", 30);
console.log(john.greet()); // "Hello, John!"
```

## Constructor

```javascript
class Car {
    constructor(brand, model, year) {
        this.brand = brand;
        this.model = model;
        this.year = year;
        this.mileage = 0;
    }
    
    drive(miles) {
        this.mileage += miles;
    }
}

const car = new Car("Toyota", "Camry", 2020);
car.drive(100);
console.log(car.mileage); // 100
```

## Static Methods

```javascript
class MathUtils {
    static add(a, b) {
        return a + b;
    }
    
    static multiply(a, b) {
        return a * b;
    }
}

MathUtils.add(5, 3); // 8
MathUtils.multiply(5, 3); // 15
```

## Getters and Setters

```javascript
class Temperature {
    constructor(celsius) {
        this._celsius = celsius;
    }
    
    get fahrenheit() {
        return this._celsius * 9/5 + 32;
    }
    
    set fahrenheit(f) {
        this._celsius = (f - 32) * 5/9;
    }
}

const temp = new Temperature(100);
console.log(temp.fahrenheit); // 212
temp.fahrenheit = 32;
console.log(temp._celsius); // 0
```

## Quick Revision

- Class = object blueprint
- `constructor()` initializes properties
- Methods defined in class body
- `static` methods belong to class
- `get`/`set` for computed properties

---

## Related Topics

- [[What-is-Class]] - Classes overview
- [[Create-Class]] - Creating classes
- [[What-is-Constructor]] - Constructors
- [[What-is-Inheritance]] - Inheritance
- [[What-is-Static]] - Static methods
- [[What-is-GetSet]] - Getters/setters
