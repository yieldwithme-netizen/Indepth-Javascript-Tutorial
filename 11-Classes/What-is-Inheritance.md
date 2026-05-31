# What is Inheritance?

## Definition

Inheritance lets a class **derive from another class**, reusing its properties and methods.

## Basic Syntax

```javascript
class Animal {
    constructor(name) {
        this.name = name;
    }
    
    speak() {
        return `${this.name} makes a noise`;
    }
}

class Dog extends Animal {
    speak() {
        return `${this.name} barks`;
    }
}

const dog = new Dog("Rex");
console.log(dog.speak()); // "Rex barks"
```

## Constructor with super

```javascript
class Animal {
    constructor(name, type) {
        this.name = name;
        this.type = type;
    }
}

class Dog extends Animal {
    constructor(name, breed) {
        super(name, "dog"); // call parent constructor
        this.breed = breed;
    }
}

const dog = new Dog("Rex", "Labrador");
console.log(dog.name);    // "Rex"
console.log(dog.type);    // "dog"
console.log(dog.breed);   // "Labrador"
```

## Method Overriding

```javascript
class Shape {
    area() {
        return 0;
    }
}

class Circle extends Shape {
    constructor(radius) {
        super();
        this.radius = radius;
    }
    
    area() {
        return Math.PI * this.radius ** 2;
    }
}

const circle = new Circle(5);
console.log(circle.area()); // 78.54
```

## Quick Revision

- `extends` creates inheritance
- `super()` calls parent constructor
- Override methods in child class
- Child inherits parent properties/methods
- Use for: code reuse, hierarchies

---

## Related Topics

- [[What-is-Inheritance]] - Inheritance overview
- [[Use-Extends]] - Using extends
- [[What-is-Super]] - super keyword
- [[What-is-Class]] - Classes
- [[Call-Parent]] - Calling parent methods
