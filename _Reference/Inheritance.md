# Inheritance

## Definition

Inheritance allows a class to **derive from another class**, reusing its properties and methods.

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

## Quick Revision

- `extends` creates inheritance
- `super()` calls parent constructor
- Override methods in child class
- Child inherits parent properties/methods

---

## Related Topics

- [[What-is-Inheritance]] - [[What-is-Inheritance|Inheritance]]
- [[Inheritance]] - [[Inheritance|Inheritance]]
- [[Use-Extends]] - [[Use-Extends|Using extends]]
- [[What-is-Class]] - [[What-is-Class|Classes]]
- [[What-is-Super]] - [[What-is-Super|super]]
