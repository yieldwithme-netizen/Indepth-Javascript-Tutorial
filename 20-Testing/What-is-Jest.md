# What is Jest

## Definition

Jest is a JavaScript testing framework developed by Meta (Facebook). It provides a complete testing solution with built-in assertions, mocking, and coverage tools. Jest is designed to work out of the box with zero configuration for most JavaScript projects.

## Why Jest

- **Zero Configuration** - Works out of the box for most projects
- **Fast Execution** - Runs tests in parallel for speed
- **Built-in Assertions** - No need for separate assertion libraries
- **Built-in Mocking** - Easy function and module mocking
- **Code Coverage** - Built-in coverage reporting
- **Snapshot Testing** - Captures and compares rendered output

## Installation

```bash
# Install as dev dependency
npm install --save-dev jest

# Or with yarn
yarn add --dev jest
```

## Basic Setup

### package.json Configuration

```json
{
  "scripts": {
    "test": "jest",
    "test:watch": "jest --watch",
    "test:coverage": "jest --coverage"
  }
}
```

### Jest Config File (jest.config.js)

```javascript
module.exports = {
  testEnvironment: 'node',
  coverageDirectory: 'coverage',
  coverageReporters: ['text', 'lcov'],
  testMatch: ['**/__tests__/**/*.js', '**/*.test.js'],
  transform: {
    '^.+\\.jsx?$': 'babel-jest'
  }
};
```

## Simple Test Example

```javascript
// math.js
function add(a, b) {
  return a + b;
}

module.exports = { add };
```

```javascript
// math.test.js
const { add } = require('./math');

test('adds 1 + 2 to equal 3', () => {
  expect(add(1, 2)).toBe(3);
});
```

## Running Tests

```bash
# Run all tests
npx jest

# Run with coverage
npx jest --coverage

# Run specific test file
npx jest math.test.js

# Run in watch mode
npx jest --watch
```

## Common Use Cases

- Unit testing functions and modules
- Integration testing APIs and components
- Regression testing after code changes
- Test-driven development (TDD)
- Continuous integration pipelines

## Common Mistakes

- Not installing Jest as devDependency
- Forgetting to configure test script in package.json
- Not mocking external dependencies
- Testing implementation instead of behavior

## Related Topics

- [[Write-UnitTests]]
- [[What-is-DescribeIt]]
- [[What-is-Matchers]]
- [[Test-Async]]
- [[What-is-Mocking]]
- [[What-is-Coverage]]
- [[Run-Tests]]

## Quick Revision

| Concept | Description |
|---------|-------------|
| Jest | JavaScript testing framework by Meta |
| Zero Config | Works out of the box |
| Assertions | Built-in expect() with matchers |
| Coverage | Built-in code coverage |
| Watch Mode | Auto-runs tests on file changes |
