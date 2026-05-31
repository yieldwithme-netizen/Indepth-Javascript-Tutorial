# Private Variables

## Definition

Private variables are **only accessible** within a class.

## Example

```javascript
class User {
    #password;
    
    constructor(name, password) {
        this.name = name;
        this.#password = password;
    }
    
    checkPassword(pwd) {
        return this.#password === pwd;
    }
}
```

## Quick Revision

- Private: `#` prefix
- Not accessible outside class
- Use for: encapsulation
- Protect sensitive data

---

## Related Topics

- [[What-is-Private]] - [[What-is-Private|Private]]
- [[Private Variables]] - [[Private Variables|Private variables]]
- [[Use-Private]] - [[Use-Private|Using private]]
- [[What-is-Encapsulation]] - [[What-is-Encapsulation|Encapsulation]]
