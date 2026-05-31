# toString

## Definition

`toString()` returns a **string representation** of an object.

## Basic Usage

```javascript
// Numbers
const num = 42;
num.toString(); // "42"
num.toString(2); // "101010" (binary)
num.toString(16); // "2a" (hex)

// Objects
const obj = { name: "John" };
obj.toString(); // "[object Object]"

// Arrays
const arr = [1, 2, 3];
arr.toString(); // "1,2,3"

// Custom
class Person {
    constructor(name) {
        this.name = name;
    }
    
    toString() {
        return `Person: ${this.name}`;
    }
}

const person = new Person("John");
console.log(person.toString()); // "Person: John"
```

## Quick Revision

- `toString()` returns string
- Numbers: specify base (2, 8, 16)
- Objects: "[object Object]"
- Custom: override `toString()` method

---

## Related Topics

- [[What-is-String]] - [[What-is-String|Strings]]
- [[toString]] - [[toString|toString]]
- [[String-Methods]] - [[String-Methods|String methods]]
- [[What-is-Method]] - [[What-is-Method|Methods]]
