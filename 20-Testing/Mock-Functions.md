# How to Mock Functions

## Definition

Mock functions are functions whose implementation has been replaced with a test double. Jest provides `jest.fn()` to create mocks and tools to verify how they're called.

## Creating Mock Functions

```javascript
// Basic mock function
const mockAdd = jest.fn();

// Mock with return value
const mockGetValue = jest.fn().mockReturnValue('test');

// Mock with implementation
const mockCalculate = jest.fn((a, b) => a + b);

// Mock with resolved value (for async)
const mockFetch = jest.fn().mockResolvedValue({ data: 'test' });
```

## Tracking Calls

```javascript
describe('Mock tracking', () => {
  test('tracks function calls', () => {
    const mockFn = jest.fn();

    mockFn('arg1');
    mockFn('arg2');

    expect(mockFn).toHaveBeenCalledTimes(2);
    expect(mockFn).toHaveBeenCalledWith('arg1');
    expect(mockFn).toHaveBeenCalledWith('arg2');
    expect(mockFn).toHaveBeenLastCalledWith('arg2');
  });

  test('tracks call arguments', () => {
    const mockFn = jest.fn();

    mockFn('a', 'b');
    mockFn('c', 'd');

    expect(mockFn.mock.calls).toEqual([
      ['a', 'b'],
      ['c', 'd']
    ]);
  });

  test('tracks return values', () => {
    const mockFn = jest.fn()
      .mockReturnValueOnce('first')
      .mockReturnValueOnce('second');

    mockFn();
    mockFn();

    expect(mockFn.mock.results).toEqual([
      { type: 'return', value: 'first' },
      { type: 'return', value: 'second' }
    ]);
  });
});
```

## Mock Return Values

```javascript
describe('Return values', () => {
  test('mockReturnValue - returns value every time', () => {
    const mockFn = jest.fn().mockReturnValue('always this');

    expect(mockFn()).toBe('always this');
    expect(mockFn()).toBe('always this');
  });

  test('mockReturnValueOnce - returns once then reverts', () => {
    const mockFn = jest.fn()
      .mockReturnValueOnce('first')
      .mockReturnValue('default');

    expect(mockFn()).toBe('first');
    expect(mockFn()).toBe('default');
    expect(mockFn()).toBe('default');
  });
});
```

## Mock Implementations

```javascript
describe('Mock implementations', () => {
  test('mockImplementation - custom logic', () => {
    const mockFn = jest.fn().mockImplementation((x) => x * 2);

    expect(mockFn(5)).toBe(10);
    expect(mockFn(10)).toBe(20);
  });

  test('mockImplementationOnce - one-time custom logic', () => {
    const mockFn = jest.fn()
      .mockImplementationOnce((x) => x + 1)
      .mockImplementation((x) => x * 2);

    expect(mockFn(5)).toBe(6);   // first call: x + 1
    expect(mockFn(5)).toBe(10);  // second call: x * 2
  });
});
```

## Mocking Async Functions

```javascript
describe('Async mocks', () => {
  test('mockResolvedValue - async return', async () => {
    const mockFetch = jest.fn().mockResolvedValue({ id: 1 });

    const result = await mockFetch();

    expect(result).toEqual({ id: 1 });
  });

  test('mockRejectedValue - async error', async () => {
    const mockFetch = jest.fn().mockRejectedValue(new Error('Network error'));

    await expect(mockFetch()).rejects.toThrow('Network error');
  });

  test('mockImplementation - async custom logic', async () => {
    const mockFn = jest.fn().mockImplementation(async (x) => {
      return x * 2;
    });

    const result = await mockFn(5);
    expect(result).toBe(10);
  });
});
```

## Mock Clearing and Resetting

```javascript
describe('Mock cleanup', () => {
  let mockFn;

  beforeEach(() => {
    mockFn = jest.fn().mockReturnValue('test');
  });

  test('clear mock - resets calls and instances', () => {
    mockFn();
    mockFn();

    expect(mockFn).toHaveBeenCalledTimes(2);

    mockFn.mockClear();

    expect(mockFn).toHaveBeenCalledTimes(0);
    expect(mockFn()).toBe('test'); // Still has mock implementation
  });

  test('reset mock - clears implementation too', () => {
    mockFn();
    mockFn();

    mockFn.mockReset();

    expect(mockFn).toHaveBeenCalledTimes(0);
    expect(mockFn()).toBeUndefined(); // No implementation
  });

  test('restore mock - removes mock entirely', () => {
    const obj = { method: () => 'original' };
    jest.spyOn(obj, 'method').mockReturnValue('mocked');

    expect(obj.method()).toBe('mocked');

    obj.method.mockRestore();

    expect(obj.method()).toBe('original');
  });
});
```

## Practical Example

```javascript
// userService.js
export async function createUser(userData) {
  const validated = validateUser(userData);
  const saved = await db.save(validated);
  await sendWelcomeEmail(saved.email);
  return saved;
}

// userService.test.js
jest.mock('./db');
jest.mock('./email');

import { createUser } from './userService';
import db from './db';
import { sendWelcomeEmail } from './email';

describe('createUser', () => {
  beforeEach(() => {
    jest.clearAllMocks();
    db.save.mockResolvedValue({ id: 1, email: 'test@test.com' });
    sendWelcomeEmail.mockResolvedValue(undefined);
  });

  test('saves user to database', async () => {
    const userData = { name: 'John', email: 'test@test.com' };

    await createUser(userData);

    expect(db.save).toHaveBeenCalledTimes(1);
    expect(db.save).toHaveBeenCalledWith(userData);
  });

  test('sends welcome email', async () => {
    const userData = { name: 'John', email: 'test@test.com' };

    await createUser(userData);

    expect(sendWelcomeEmail).toHaveBeenCalledWith('test@test.com');
  });
});
```

## Common Use Cases

- Isolating code from external services
- Testing error handling paths
- Verifying function calls
- Testing conditional logic
- Simulating different scenarios

## Common Mistakes

- Not clearing mocks between tests
- Over-mocking what should be tested
- Not verifying important mock calls
- Forgetting to handle async mocks
- Using mockReturnValue for complex logic

## Related Topics

- [[What-is-Mocking]]
- [[What-is-Jest]]
- [[Test-Async]]
- [[Write-UnitTests]]

## Quick Revision

| Method | Purpose |
|--------|---------|
| jest.fn() | Create mock function |
| mockReturnValue | Set return value |
| mockImplementation | Custom logic |
| mockResolvedValue | Async return |
| mockRejectedValue | Async error |
| mockClear | Reset calls only |
| mockReset | Clear implementation |
| mockRestore | Remove mock |
