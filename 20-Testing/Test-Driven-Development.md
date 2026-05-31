# Test-Driven Development

## Definition

TDD writes **tests before code**.

## Workflow

```javascript
// 1. Write failing test
test('adds 1 + 2', () => {
    expect(add(1, 2)).toBe(3);
});

// 2. Write code to pass
function add(a, b) { return a + b; }

// 3. Refactor
```

## Quick Revision

- TDD = test first, code second
- Red → Green → Refactor
- Write failing test
- Make test pass

---

## Related Topics

- [[What-is-TDD]] - [[What-is-TDD|TDD]]
- [[TDD]] - [[TDD|TDD]]
- [[Test-Driven-Development]] - [[Test-Driven-Development|TDD]]
- [[Write-UnitTests]] - [[Write-UnitTests|Writing tests]]
