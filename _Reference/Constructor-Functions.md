# Constructor Functions

## Definition

Constructor functions create **new objects** using `new`.

## Basic Syntax

```javascript
function Person(name, age) {
    this.name = name;
    this.age = age;
    this.greet = function() {
        return `Hello, ${this.name}!`;
    };
}

const john = new Person("John", 30);
console.log(john.greet()); // "Hello, John!"
```

## Quick Revision

- Constructor: function with `new`
- `this` refers to new object
- Initialize properties in constructor
- Use classes instead (modern)

---

## Related Topics

- [[What-is-Constructor]] - [[What-is-Constructor|Constructors]]
- [[Constructor-Functions]] - [[Constructor-Functions|Constructor functions]]
- [[What-is-Class]] - [[What-is-Class|Classes]]
- [[What-is-Object]] - [[What-is-Object|Objects]]
