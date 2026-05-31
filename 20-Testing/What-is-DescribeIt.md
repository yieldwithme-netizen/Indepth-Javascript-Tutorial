# What is Describe/It Blocks

## Definition

`describe` and `it` (or `test`) blocks are Jest's way of organizing and grouping tests. `describe` creates test suites, while `it`/`test` defines individual test cases. They provide structure and context to your tests.

## Basic Syntax

```javascript
// describe() - groups related tests
// it() or test() - defines individual test cases

describe('Calculator', () => {
  it('should add two numbers', () => {
    expect(add(1, 2)).toBe(3);
  });

  it('should subtract two numbers', () => {
    expect(subtract(5, 3)).toBe(2);
  });
});
```

## Nested Describe Blocks

```javascript
describe('User', () => {
  describe('validation', () => {
    it('should require a name', () => {
      expect(validateUser({})).toHaveProperty('name');
    });

    it('should require an email', () => {
      expect(validateUser({ name: 'John' })).toHaveProperty('email');
    });
  });

  describe('permissions', () => {
    it('should allow read access', () => {
      expect(user.canRead()).toBe(true);
    });

    it('should deny admin access', () => {
      expect(user.canAdmin()).toBe(false);
    });
  });
});
```

## Using test() vs it()

```javascript
// These are equivalent
test('adds numbers', () => {
  expect(add(1, 2)).toBe(3);
});

it('adds numbers', () => {
  expect(add(1, 2)).toBe(3);
});

// Best practice: use 'test' for behavior, 'it' for specifications
test('calculates total', () => { ... });
it('should return the sum of items', () => { ... });
```

## Setup and Teardown Hooks

```javascript
describe('Database Tests', () => {
  beforeAll(() => {
    // Run once before all tests
    connectToDatabase();
  });

  beforeEach(() => {
    // Run before each test
    clearDatabase();
    seedTestData();
  });

  afterEach(() => {
    // Run after each test
    cleanupTestData();
  });

  afterAll(() => {
    // Run once after all tests
    closeDatabaseConnection();
  });

  it('should fetch user', () => { ... });
  it('should create user', () => { ... });
});
```

## Pending Tests

```javascript
describe('Feature', () => {
  it('should work', () => {
    expect(true).toBe(true);
  });

  // Skip a test
  it.skip('should be implemented later', () => {
    // Not yet implemented
  });

  // Todo test
  it.todo('should handle edge cases');
});
```

## Common Use Cases

- Grouping related tests together
- Setting up shared test context
- Organizing test suites logically
- Using hooks for setup/teardown
- Marking pending or skipped tests

## Common Mistakes

- Nesting describe blocks too deeply
- Not using before/after hooks appropriately
- Inconsistent use of test vs it
- Not describing behavior clearly in test names
- Putting unrelated tests in same describe block

## Related Topics

- [[What-is-Jest]]
- [[Write-UnitTests]]
- [[What-is-Matchers]]
- [[Run-Tests]]

## Quick Revision

| Block | Purpose |
|-------|---------|
| describe | Groups related tests |
| it/test | Defines individual test case |
| beforeAll | Runs once before all tests |
| beforeEach | Runs before each test |
| afterEach | Runs after each test |
| afterAll | Runs once after all tests |
