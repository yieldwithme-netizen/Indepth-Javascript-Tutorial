# Encapsulation

## Definition

Encapsulation **hides internal details** and exposes only necessary functionality.

## Example

```javascript
class BankAccount {
    #balance = 0;
    
    deposit(amount) {
        this.#balance += amount;
    }
    
    getBalance() {
        return this.#balance;
    }
}
```

## Quick Revision

- Hide internal state
- Expose public interface
- Use private fields (#)
- Protect data integrity

---

## Related Topics

- [[What-is-Encapsulation]] - [[What-is-Encapsulation|Encapsulation]]
- [[Encapsulation]] - [[Encapsulation|Encapsulation]]
- [[Implement-Encapsulation]] - [[Implement-Encapsulation|Implementing encapsulation]]
- [[What-is-Class]] - [[What-is-Class|Classes]]
