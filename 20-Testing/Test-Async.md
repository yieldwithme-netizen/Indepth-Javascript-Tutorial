# How to Test Async Code

## Definition

Testing asynchronous code requires special handling because async operations (Promises, async/await, callbacks) don't complete immediately. Jest provides multiple ways to handle async testing.

## Testing Promises with async/await

```javascript
async function fetchUser(id) {
  const response = await fetch(`/api/users/${id}`);
  return response.json();
}

describe('fetchUser', () => {
  test('returns user data', async () => {
    const user = await fetchUser(1);
    expect(user).toHaveProperty('name');
    expect(user.id).toBe(1);
  });

  test('throws error for invalid id', async () => {
    await expect(fetchUser(null)).rejects.toThrow();
  });
});
```

## Testing Promises with .resolves/.rejects

```javascript
describe('fetchUser with .resolves', () => {
  test('returns user data', () => {
    return expect(fetchUser(1)).resolves.toHaveProperty('name');
  });

  test('rejects for invalid id', () => {
    return expect(fetchUser(null)).rejects.toThrow();
  });
});
```

## Testing Callbacks

```javascript
function fetchData(callback) {
  setTimeout(() => {
    callback(null, { data: 'test' });
  }, 1000);
}

describe('fetchData', () => {
  test('calls callback with data', (done) => {
    fetchData((err, data) => {
      expect(err).toBeNull();
      expect(data).toEqual({ data: 'test' });
      done(); // Must call done() to signal test completion
    });
  });
});
```

## Testing with Mock Timers

```javascript
describe('setTimeout tests', () => {
  beforeEach(() => {
    jest.useFakeTimers();
  });

  afterEach(() => {
    jest.useRealTimers();
  });

  test('calls function after delay', () => {
    const callback = jest.fn();
    setTimeout(callback, 1000);

    expect(callback).not.toHaveBeenCalled();

    jest.advanceTimersByTime(1000);

    expect(callback).toHaveBeenCalledTimes(1);
  });
});
```

## Testing Async Functions with Errors

```javascript
async function processUser(id) {
  const user = await fetchUser(id);
  if (!user.active) {
    throw new Error('User is not active');
  }
  return user;
}

describe('processUser', () => {
  test('throws for inactive user', async () => {
    fetchUser.mockResolvedValue({ active: false });

    await expect(processUser(1)).rejects.toThrow('User is not active');
  });

  test('returns active user', async () => {
    const mockUser = { id: 1, active: true };
    fetchUser.mockResolvedValue(mockUser);

    const result = await processUser(1);
    expect(result).toEqual(mockUser);
  });
});
```

## Testing API Calls

```javascript
// Using fetch mock
global.fetch = jest.fn(() =>
  Promise.resolve({
    json: () => Promise.resolve({ data: 'test' })
  })
);

describe('API calls', () => {
  beforeEach(() => {
    fetch.mockClear();
  });

  test('makes API call', async () => {
    await getData();

    expect(fetch).toHaveBeenCalledTimes(1);
    expect(fetch).toHaveBeenCalledWith('/api/data');
  });
});
```

## Common Use Cases

- Testing API calls
- Testing database operations
- Testing file system operations
- Testing user interactions
- Testing timeouts and delays

## Common Mistakes

- Forgetting to return promise or use async/await
- Not calling `done()` in callback tests
- Not mocking timers for time-dependent code
- Not cleaning up mocks between tests
- Not handling rejected promises

## Related Topics

- [[What-is-Jest]]
- [[What-is-Mocking]]
- [[Mock-Functions]]
- [[Write-UnitTests]]

## Quick Revision

| Method | Use Case |
|--------|----------|
| async/await | Modern promise handling |
| .resolves/.rejects | Promise assertion |
| done() | Callback-based tests |
| fakeTimers | Time-dependent code |
