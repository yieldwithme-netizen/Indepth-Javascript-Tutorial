# Testing in JavaScript

## Definition

Testing is the practice of verifying that code behaves as expected. It involves writing automated tests that check functionality, catch regressions, and ensure code quality throughout development.

## Types of Testing

### 1. Unit Testing

Tests individual functions or components in isolation.

```javascript
// Function to test
function add(a, b) {
  return a + b;
}

// Unit test
describe('add function', () => {
  test('adds two positive numbers', () => {
    expect(add(2, 3)).toBe(5);
  });

  test('adds negative numbers', () => {
    expect(add(-1, -2)).toBe(-3);
  });

  test('handles zero', () => {
    expect(add(5, 0)).toBe(5);
  });
});
```

### 2. Integration Testing

Tests how multiple components work together.

```javascript
// Testing API integration
describe('User API', () => {
  test('creates and retrieves user', async () => {
    const user = await createUser({ name: 'John' });
    const retrieved = await getUser(user.id);
    expect(retrieved.name).toBe('John');
  });
});
```

### 3. End-to-End (E2E) Testing

Tests complete user workflows.

```javascript
// Cypress or Playwright example
describe('Login flow', () => {
  test('user can log in successfully', () => {
    cy.visit('/login');
    cy.get('[data-cy=email]').type('user@example.com');
    cy.get('[data-cy=password]').type('password123');
    cy.get('[data-cy=submit]').click();
    cy.url().should('include', '/dashboard');
  });
});
```

## Popular Testing Frameworks

### Jest

```javascript
// sum.test.js
function sum(a, b) {
  return a + b;
}

module.exports = sum;

test('adds 1 + 2 to equal 3', () => {
  expect(sum(1, 2)).toBe(3);
});
```

### Mocha + Chai

```javascript
const { expect } = require('chai');
const sum = require('./sum');

describe('Sum function', () => {
  it('should add two numbers correctly', () => {
    expect(sum(1, 2)).to.equal(3);
  });
});
```

### Vitest

```javascript
import { describe, it, expect } from 'vitest';
import { sum } from './sum';

describe('sum', () => {
  it('adds two numbers', () => {
    expect(sum(1, 2)).toBe(3);
  });
});
```

## Test Structure

### Arrange-Act-Assert (AAA)

```javascript
test('calculates total price with tax', () => {
  // Arrange
  const price = 100;
  const taxRate = 0.1;

  // Act
  const total = calculateTotal(price, taxRate);

  // Assert
  expect(total).toBe(110);
});
```

### Test Doubles

```javascript
// Mock function
const mockFetch = jest.fn();
mockFetch.mockResolvedValue({ data: 'test' });

// Spy
const spy = jest.spyOn(console, 'log');

// Stub
jest.stubOn(Math, 'random').mockReturnValue(0.5);
```

## Common Testing Patterns

### Testing Async Code

```javascript
// Callbacks
test('fetches data with callback', (done) => {
  fetchData((result) => {
    expect(result).toBe('expected');
    done();
  });
});

// Promises
test('fetches data with promise', () => {
  return fetchData().then(result => {
    expect(result).toBe('expected');
  });
});

// Async/Await
test('fetches data with async/await', async () => {
  const result = await fetchData();
  expect(result).toBe('expected');
});
```

### Testing Exceptions

```javascript
test('throws error for invalid input', () => {
  expect(() => {
    divide(10, 0);
  }).toThrow('Cannot divide by zero');
});
```

### Testing DOM Manipulation

```javascript
// Using jsdom
test('updates element text', () => {
  document.body.innerHTML = '<div id="app"></div>';
  
  updateText('app', 'Hello World');
  
  expect(document.getElementById('app').textContent)
    .toBe('Hello World');
});
```

## Test Coverage

```javascript
// Jest coverage configuration
// jest.config.js
module.exports = {
  collectCoverage: true,
  coverageThreshold: {
    global: {
      branches: 80,
      functions: 80,
      lines: 80,
      statements: 80
    }
  }
};
```

## Best Practices

```javascript
// ✅ Good: Descriptive test names
test('returns empty array when no items match filter', () => {});

// ❌ Bad: Vague test names
test('filter works', () => {});

// ✅ Good: One assertion per test concept
test('validates email format', () => {
  expect(validateEmail('valid@example.com')).toBe(true);
  expect(validateEmail('invalid')).toBe(false);
});

// ✅ Good: Independent tests
test('case 1', () => {
  const result = functionA();
  expect(result).toBe(expected);
});

test('case 2', () => {
  const result = functionB();
  expect(result).toBe(expected);
});
```

## Common Mistakes

```javascript
// ❌ Testing implementation details
test('calls internal method', () => {
  const spy = jest.spyOn(obj, 'internalMethod');
  obj.publicMethod();
  expect(spy).toHaveBeenCalled();
});

// ✅ Testing behavior
test('produces correct output', () => {
  const result = obj.publicMethod();
  expect(result).toBe(expectedOutput);
});

// ❌ Tests that depend on each other
test('step 1', () => { /* ... */ });
test('step 2', () => { /* depends on step 1 */ });

// ✅ Independent, isolated tests
test('independent case 1', () => { /* ... */ });
test('independent case 2', () => { /* ... */ });
```

## Testing Tools

| Tool | Type | Best For |
|------|------|----------|
| Jest | Unit/Integration | React, Node.js |
| Mocha | Unit/Integration | Flexible setup |
| Vitest | Unit/Integration | Vite projects |
| Cypress | E2E | Web applications |
| Playwright | E2E | Cross-browser |
| Testing Library | Component | React, Vue, DOM |

## Related Topics

- [[Debugging]] - Finding and fixing issues
- [[Code-Review]] - Peer code examination
- [[CI-CD]] - Automated testing pipelines
- [[Mocking]] - Test doubles and stubs
- [[TDD]] - Test-Driven Development
- [[BDD]] - Behavior-Driven Development

## Quick Revision

**Testing Types:**
- **Unit**: Individual functions
- **Integration**: Multiple components
- **E2E**: Complete workflows

**Key Principles:**
- Test behavior, not implementation
- Keep tests independent
- Write descriptive test names
- Aim for meaningful coverage
- Test edge cases and errors

**Framework Selection:**
- Jest: Most popular, batteries included
- Vitest: Modern, fast, Vite-compatible
- Mocha: Flexible, minimal defaults
- Playwright: Cross-browser E2E