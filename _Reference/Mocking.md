# Mocking

## Definition

Mocking **replaces real implementations** with fake ones for testing.

## Basic Example

```javascript
// Jest mock
const mockFetch = jest.fn();
mockFetch.mockResolvedValue({ data: "test" });

// Manual mock
const mockAPI = {
    getUsers: () => [{ id: 1, name: "John" }]
};
```

## Quick Revision

- Mock: fake implementation for testing
- Use jest.fn() for function mocks
- Mock returns for simulating responses
- Restore after tests

---

## Related Topics

- [[What-is-Mocking]] - [[What-is-Mocking|Mocking]]
- [[Mocking]] - [[Mocking|Mocking]]
- [[Mock-Functions]] - [[Mock-Functions|Mock functions]]
- [[What-is-Jest]] - [[What-is-Jest|Jest]]
