# Mocha

## Definition

Mocha is a **JavaScript testing framework**.

## Example

```javascript
describe('Array', function() {
    describe('#indexOf()', function() {
        it('should return -1 when not found', function() {
            assert.strictEqual([1, 2, 3].indexOf(4), -1);
        });
    });
});
```

## Quick Revision

- Mocha = testing framework
- describe() for groups
- it() for tests
- Use with assertion libraries

---

## Related Topics

- [[What-is-Mocha]] - [[What-is-Mocha|Mocha]]
- [[Mocha]] - [[Mocha|Mocha]]
- [[What-is-Jest]] - [[What-is-Jest|Jest]]
- [[Write-UnitTests]] - [[Write-UnitTests|Writing tests]]
