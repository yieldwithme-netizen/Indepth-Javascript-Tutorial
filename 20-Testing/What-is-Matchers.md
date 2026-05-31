# What are Matchers

## Definition

Matchers are functions used in Jest's `expect()` to test specific conditions. They compare the actual result against an expected value and determine if the test passes or fails.

## Common Matchers

```javascript
describe('Common Matchers', () => {
  // Equality
  test('toBe - strict equality', () => {
    expect(2 + 2).toBe(4);
    expect('hello').toBe('hello');
  });

  test('toEqual - deep equality', () => {
    expect({ a: 1, b: 2 }).toEqual({ a: 1, b: 2 });
    expect([1, 2, 3]).toEqual([1, 2, 3]);
  });

  // Truthiness
  test('toBeTruthy - any truthy value', () => {
    expect(true).toBeTruthy();
    expect(1).toBeTruthy();
    expect('text').toBeTruthy();
  });

  test('toBeFalsy - any falsy value', () => {
    expect(false).toBeFalsy();
    expect(0).toBeFalsy();
    expect('').toBeFalsy();
  });

  test('toBeNull', () => {
    expect(null).toBeNull();
  });

  test('toBeUndefined', () => {
    expect(undefined).toBeUndefined();
  });

  test('toBeDefined', () => {
    expect({}).toBeDefined();
  });
});
```

## Numeric Matchers

```javascript
describe('Numeric Matchers', () => {
  test('toBeGreaterThan', () => {
    expect(10).toBeGreaterThan(5);
  });

  test('toBeGreaterThanOrEqual', () => {
    expect(10).toBeGreaterThanOrEqual(10);
  });

  test('toBeLessThan', () => {
    expect(5).toBeLessThan(10);
  });

  test('toBeLessThanOrEqual', () => {
    expect(5).toBeLessThanOrEqual(5);
  });

  test('toBeCloseTo - floating point', () => {
    expect(0.1 + 0.2).toBeCloseTo(0.3);
  });
});
```

## String Matchers

```javascript
describe('String Matchers', () => {
  test('toMatch - regex match', () => {
    expect('hello world').toMatch(/world/);
    expect('hello world').toMatch('hello');
  });

  test('string containing', () => {
    expect('JavaScript').toContain('Script');
  });
});
```

## Array and Iterable Matchers

```javascript
describe('Array Matchers', () => {
  test('toContain - array contains item', () => {
    expect([1, 2, 3]).toContain(2);
    expect(['a', 'b']).toContain('b');
  });

  test('toHaveLength', () => {
    expect([1, 2, 3]).toHaveLength(3);
    expect('hello').toHaveLength(5);
  });
});
```

## Object Matchers

```javascript
describe('Object Matchers', () => {
  test('toHaveProperty', () => {
    const user = { name: 'John', age: 30 };
    expect(user).toHaveProperty('name');
    expect(user).toHaveProperty('age', 30);
  });

  test('toHaveBeenCalledTimes', () => {
    const mockFn = jest.fn();
    mockFn();
    mockFn();
    expect(mockFn).toHaveBeenCalledTimes(2);
  });
});
```

## Negating Matchers

```javascript
describe('Negating Matchers', () => {
  test('not.toBe', () => {
    expect(2 + 2).not.toBe(5);
  });

  test('not.toEqual', () => {
    expect({ a: 1 }).not.toEqual({ b: 2 });
  });

  test('not.toContain', () => {
    expect([1, 2]).not.toContain(3);
  });
});
```

## Custom Matchers

```javascript
expect.extend({
  toBeWithinRange(received, floor, ceiling) {
    const pass = received >= floor && received <= ceiling;
    return {
      pass,
      message: () =>
        `expected ${received} ${pass ? 'not ' : ''}to be within range ${floor} - ${ceiling}`
    };
  }
});

test('number is within range', () => {
  expect(50).toBeWithinRange(1, 100);
});
```

## Common Use Cases

- Testing function return values
- Verifying object properties
- Checking array contents
- Validating string patterns
- Asserting error conditions

## Common Mistakes

- Using `toBe` for objects (use `toEqual` instead)
- Not using `toBeCloseTo` for floating point
- Forgetting negation with `.not`
- Using wrong matcher for data type
- Not reading error messages carefully

## Related Topics

- [[What-is-Jest]]
- [[Write-UnitTests]]
- [[What-is-DescribeIt]]

## Quick Revision

| Matcher | Purpose |
|---------|---------|
| toBe | Strict equality (===) |
| toEqual | Deep equality |
| toContain | Array/string contains |
| toHaveLength | Array/string length |
| toThrow | Function throws error |
| not | Negate any matcher |
