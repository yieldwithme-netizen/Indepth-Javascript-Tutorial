# How to Write Unit Tests

## Definition

Unit tests are small, focused tests that verify individual functions, methods, or components work correctly in isolation. Each test should check one specific behavior or output.

## Basic Structure

```javascript
// Function to test
function multiply(a, b) {
  return a * b;
}

// Test file: multiply.test.js
describe('multiply', () => {
  test('multiplies two positive numbers', () => {
    expect(multiply(2, 3)).toBe(6);
  });

  test('returns zero when multiplied by zero', () => {
    expect(multiply(5, 0)).toBe(0);
  });

  test('handles negative numbers', () => {
    expect(multiply(-2, 3)).toBe(-6);
  });
});
```

## AAA Pattern (Arrange, Act, Assert)

```javascript
test('calculates total with tax', () => {
  // Arrange
  const price = 100;
  const taxRate = 0.1;

  // Act
  const total = calculateTotal(price, taxRate);

  // Assert
  expect(total).toBe(110);
});
```

## Testing Different Data Types

```javascript
describe('Testing different types', () => {
  test('returns correct string', () => {
    expect(greet('World')).toBe('Hello, World!');
  });

  test('returns correct array', () => {
    expect(getNumbers()).toEqual([1, 2, 3]);
  });

  test('returns correct object', () => {
    expect(getUser()).toEqual({
      name: 'John',
      age: 30
    });
  });

  test('returns true boolean', () => {
    expect(isEven(4)).toBe(true);
  });
});
```

## Edge Cases

```javascript
describe('Edge cases', () => {
  test('handles empty string', () => {
    expect(validateEmail('')).toBe(false);
  });

  test('handles null input', () => {
    expect(formatName(null)).toBe('');
  });

  test('handles undefined input', () => {
    expect(formatName(undefined)).toBe('');
  });

  test('handles large numbers', () => {
    expect(add(Number.MAX_SAFE_INTEGER, 1)).toBeDefined();
  });
});
```

## Testing Error Cases

```javascript
describe('Error handling', () => {
  test('throws error for negative age', () => {
    expect(() => setAge(-1)).toThrow('Age cannot be negative');
  });

  test('throws error for invalid type', () => {
    expect(() => divide(10, 'a')).toThrow(TypeError);
  });
});
```

## Common Use Cases

- Testing utility functions
- Testing data transformations
- Testing validation logic
- Testing business rules
- Testing pure functions

## Common Mistakes

- Testing too many things in one test
- Not testing edge cases
- Testing implementation details
- Not cleaning up after tests
- Writing brittle tests that break easily

## Related Topics

- [[What-is-Jest]]
- [[What-is-DescribeIt]]
- [[What-is-Matchers]]
- [[What-is-Mocking]]

## Quick Revision

| Pattern | Description |
|---------|-------------|
| AAA | Arrange, Act, Assert |
| Edge Cases | Null, undefined, empty values |
| Error Testing | expect().toThrow() |
| One Assertion | Each test checks one behavior |
