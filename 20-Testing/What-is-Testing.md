# What is Testing?

## Definition

Testing is **verifying that code works correctly**. It catches bugs early and ensures reliability.

## Types of Testing

| Type | Tests | Example |
|------|-------|---------|
| Unit | Individual functions | `add(1, 2) === 3` |
| Integration | Multiple components | API + Database |
| End-to-End | Full application | Browser automation |

## Testing Frameworks

| Framework | Description |
|-----------|-------------|
| Jest | Most popular, full-featured |
| Mocha | Flexible, needs plugins |
| Vitest | Fast, Vite-based |
| Cypress | E2E testing |
| Playwright | Browser automation |

## Basic Test

```javascript
// add.test.js
function add(a, b) {
    return a + b;
}

test('adds 1 and 2 to equal 3', () => {
    expect(add(1, 2)).toBe(3);
});
```

## Why Test?

- Catch bugs early
- Refactor safely
- Document behavior
- Improve code quality
- Enable CI/CD

## Quick Revision

- Testing = verifying code works
- Unit: test individual functions
- Integration: test components together
- E2E: test full application
- Use Jest for JavaScript

---

## Related Topics

- [[What-is-Testing]] - Testing overview
- [[What-is-Jest]] - Jest framework
- [[Write-UnitTests]] - Writing tests
- [[What-is-DescribeIt]] - Describe/it blocks
- [[What-is-Matchers]] - Assertions
