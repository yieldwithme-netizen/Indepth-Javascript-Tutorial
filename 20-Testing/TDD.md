# TDD (Test-Driven Development)

## Definition

TDD writes **tests before code**.

## Workflow

```javascript
// 1. Write failing test
test('adds 1 + 2', () => {
    expect(add(1, 2)).toBe(3);
});

// 2. Write minimal code to pass
function add(a, b) { return a + b; }

// 3. Refactor
```

## Quick Revision

- TDD = test first, code second
- Red → Green → Refactor
- Write failing test
- Make test pass
- Refactor code

---

## Related Topics

- [[What-is-TDD]] - [[What-is-TDD|TDD]]
- [[TDD]] - [[TDD|TDD]]
- [[Write-UnitTests]] - [[Write-UnitTests|Writing tests]]
- [[What-is-Jest]] - [[What-is-Jest|Jest]]
