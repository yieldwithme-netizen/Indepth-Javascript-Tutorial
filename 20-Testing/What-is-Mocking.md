# What is Mocking

## Definition

Mocking is replacing real dependencies (functions, modules, APIs) with fake implementations during tests. This isolates the code being tested and prevents side effects like network calls or database writes.

## Why Mock

- **Isolation** - Test code without external dependencies
- **Speed** - Avoid slow operations like API calls
- **Control** - Simulate specific scenarios and errors
- **Determinism** - Remove randomness from tests
- **Protection** - Prevent real side effects

## Jest Mock Functions

```javascript
// Create a mock function
const mockFn = jest.fn();

// Mock with return values
mockFn.mockReturnValue(42);
mockFn.mockReturnValueOnce('first call');
mockFn.mockReturnValueOnce('second call');
```

## Mocking Modules

```javascript
// api.js
export async function fetchUser(id) {
  const response = await fetch(`/api/users/${id}`);
  return response.json();
}

// user.test.js
jest.mock('./api');

import { fetchUser } from './api';
import { processUser } from './user';

test('processes user data', async () => {
  fetchUser.mockResolvedValue({ id: 1, name: 'John' });

  const result = await processUser(1);
  expect(result).toBe('John');
});
```

## Mocking Object Methods

```javascript
const userService = {
  save: async (user) => { /* database call */ },
  validate: (user) => { /* validation logic */ }
};

describe('saveUser', () => {
  beforeEach(() => {
    jest.spyOn(userService, 'save').mockResolvedValue(true);
  });

  afterEach(() => {
    jest.restoreAllMocks();
  });

  test('calls save with user', async () => {
    await saveUser({ name: 'John' });

    expect(userService.save).toHaveBeenCalledTimes(1);
    expect(userService.save).toHaveBeenCalledWith({ name: 'John' });
  });
});
```

## Mock Implementation

```javascript
const mockFn = jest.fn();

// Mock implementation
mockFn.mockImplementation((x) => x * 2);

// Mock implementation for specific args
mockFn.mockImplementationOnce((x) => x + 1);
mockFn.mockImplementationOnce((x) => x * 3);

test('uses mock implementation', () => {
  expect(mockFn(5)).toBe(10);  // 5 * 2
  expect(mockFn(5)).toBe(6);   // 5 + 1
  expect(mockFn(5)).toBe(15);  // 5 * 3
});
```

## Common Use Cases

- Mocking API calls
- Mocking database operations
- Mocking file system operations
- Mocking date/time
- Mocking random values

## Common Mistakes

- Not clearing mocks between tests
- Over-mocking (hiding real bugs)
- Mocking too many things
- Not restoring original implementations
- Not verifying mock calls

## Related Topics

- [[Mock-Functions]]
- [[What-is-Jest]]
- [[Write-UnitTests]]
- [[Test-Async]]

## Quick Revision

| Technique | Purpose |
|-----------|---------|
| jest.fn() | Create mock function |
| jest.mock() | Mock entire module |
| jest.spyOn() | Spy and mock method |
| mockResolvedValue | Mock async return |
| mockImplementation | Custom mock logic |
