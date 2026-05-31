# Unit Testing

## Definition

Unit testing tests **individual functions** in isolation.

## Example

```javascript
// Function
function add(a, b) { return a + b; }

// Test
test('adds 1 + 2', () => {
    expect(add(1, 2)).toBe(3);
});
```

## Quick Revision

- Unit test = test single function
- Test in isolation
- Use mocks for dependencies
- Jest for testing

---

## Related Topics

- [[What-is-Testing]] - [[What-is-Testing|Testing]]
- [[Unit-Testing]] - [[Unit-Testing|Unit testing]]
- [[Write-UnitTests]] - [[Write-UnitTests|Writing tests]]
- [[What-is-Jest]] - [[What-is-Jest|Jest]]
