# Getters and Setters

## Definition

Getters and setters **control property access**.

## Example

```javascript
class Person {
    #name;
    
    constructor(name) {
        this.#name = name;
    }
    
    get name() {
        return this.#name;
    }
    
    set name(value) {
        if (value.length < 2) {
            throw new Error("Name too short");
        }
        this.#name = value;
    }
}
```

## Quick Revision

- get: retrieve property value
- set: modify property value
- Use for validation
- Encapsulate logic

---

## Related Topics

- [[What-is-GetSet]] - [[What-is-GetSet|Getters/setters]]
- [[GettersSetters]] - [[GettersSetters|Getters/setters]]
- [[Use-GetSet]] - [[Use-GetSet|Using getters/setters]]
- [[What-is-Class]] - [[What-is-Class|Classes]]
