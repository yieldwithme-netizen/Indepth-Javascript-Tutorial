# How to Run Tests

## Definition

Running tests executes your test files to verify code works correctly. Jest provides various commands to run tests in different modes and configurations.

## Basic Commands

```bash
# Run all tests
npx jest

# Run with npm script
npm test

# Run with yarn
yarn test
```

## Running Specific Tests

```bash
# Run specific test file
npx jest math.test.js

# Run tests matching pattern
npx jest --testNamePattern="adds"

# Run tests in specific directory
npx jest src/__tests__/
```

## Watch Mode

```bash
# Watch all tests
npx jest --watch

# Watch specific file
npx jest --watch math.test.js

# Watch with coverage
npx jest --watch --coverage
```

## Coverage Reports

```bash
# Generate coverage report
npx jest --coverage

# Text output (console)
npx jest --coverage --coverageReporters="text"

# HTML report
npx jest --coverage --coverageReporters="html"

# LCOV (for CI tools)
npx jest --coverage --coverageReporters="lcov"
```

## CI/CD Configuration

```json
// package.json
{
  "scripts": {
    "test": "jest",
    "test:ci": "jest --ci --coverage --coverageReporters=lcov",
    "test:watch": "jest --watch",
    "test:debug": "node --inspect-brk node_modules/.bin/jest --runInBand"
  }
}
```

## GitHub Actions Example

```yaml
# .github/workflows/test.yml
name: Tests

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: '18'
      - run: npm ci
      - run: npm test
      - run: npm run test:coverage
```

## Debugging Tests

```bash
# Debug specific test
node --inspect-brk node_modules/.bin/jest --runInBand math.test.js

# Debug with Chrome DevTools
node --inspect-brk node_modules/.bin/jest --runInBand
```

## Jest CLI Options

```bash
# Verbose output
npx jest --verbose

# Run in band (serial)
npx jest --runInBand

# Force exit after tests
npx jest --forceExit

# Detect open handles
npx jest --detectOpenHandles

# Show config
npx jest --showConfig
```

## Environment Variables

```bash
# Set test environment
NODE_ENV=test jest

# Verbose logs
DEBUG=jest:* jest

# Custom env file
jest --env=node
```

## Common Use Cases

- Local development testing
- CI/CD pipelines
- Pre-commit hooks
- Code review verification
- Regression testing

## Common Mistakes

- Not running tests before committing
- Ignoring test failures in CI
- Not using watch mode during development
- Running all tests instead of affected ones
- Not setting up proper CI configuration

## Related Topics

- [[What-is-Jest]]
- [[What-is-Coverage]]
- [[Write-UnitTests]]

## Quick Revision

| Command | Purpose |
|---------|---------|
| jest | Run all tests |
| jest --watch | Watch mode |
| jest --coverage | With coverage |
| jest file.test.js | Run specific file |
| jest --runInBand | Serial execution |
| jest --ci | CI mode |
