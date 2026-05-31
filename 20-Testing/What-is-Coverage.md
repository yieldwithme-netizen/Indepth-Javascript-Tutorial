# What is Test Coverage

## Definition

Test coverage measures how much of your code is executed during tests. It helps identify untested code paths and ensures your tests provide adequate protection against bugs.

## Coverage Metrics

```javascript
// Four main types of coverage:

// 1. Statement Coverage - Are all statements executed?
function isPositive(n) {
  if (n > 0) return true;   // Statement 1
  return false;              // Statement 2
}

// 2. Branch Coverage - Are all branches taken?
function isEven(n) {
  if (n % 2 === 0) {        // Branch 1 (true)
    return true;
  } else {                    // Branch 2 (false)
    return false;
  }
}

// 3. Function Coverage - Are all functions called?
function add(a, b) { return a + b; }      // Function 1
function subtract(a, b) { return a - b; } // Function 2

// 4. Line Coverage - Are all lines executed?
```

## Running Coverage

```bash
# Run tests with coverage
npx jest --coverage

# Run with specific coverage thresholds
npx jest --coverage --coverageThreshold='{"global":{"branches":80,"functions":80}}'

# Generate coverage report
npx jest --coverage --coverageReporters="text" --coverageReporters="lcov"
```

## Coverage Configuration

```javascript
// jest.config.js
module.exports = {
  coverageDirectory: 'coverage',
  collectCoverageFrom: [
    'src/**/*.js',
    '!src/**/*.test.js',
    '!src/**/*.spec.js',
    '!src/**/index.js'
  ],
  coverageThreshold: {
    global: {
      branches: 80,
      functions: 80,
      lines: 80,
      statements: 80
    }
  },
  coverageReporters: ['text', 'lcov', 'html']
};
```

## Coverage Report Example

```
--------------------------|----------|----------|----------|----------|
File                      |  % Stmts | % Branch |  % Funcs |  % Lines |
--------------------------|----------|----------|----------|----------|
All files                 |     85.7 |     75.0 |     90.0 |     85.7 |
  src/math.js             |    100.0 |    100.0 |    100.0 |    100.0 |
  src/utils.js            |     75.0 |     50.0 |     80.0 |     75.0 |
--------------------------|----------|----------|----------|----------|
```

## Coverage Badges

```json
// package.json
{
  "scripts": {
    "test:coverage": "jest --coverage && coverage-badge"
  }
}
```

## What Good Coverage Looks Like

```javascript
// 100% coverage for this function
function calculateGrade(score) {
  if (score >= 90) return 'A';
  if (score >= 80) return 'B';
  if (score >= 70) return 'C';
  if (score >= 60) return 'D';
  return 'F';
}

// Tests achieving 100% branch coverage
test('returns A for 90+', () => expect(calculateGrade(95)).toBe('A'));
test('returns B for 80-89', () => expect(calculateGrade(85)).toBe('B'));
test('returns C for 70-79', () => expect(calculateGrade(75)).toBe('C'));
test('returns D for 60-69', () => expect(calculateGrade(65)).toBe('D'));
test('returns F for below 60', () => expect(calculateGrade(50)).toBe('F'));
```

## Common Use Cases

- Identifying untested code
- Setting quality gates in CI/CD
- Tracking test progress
- Ensuring critical paths are tested
- Meeting compliance requirements

## Common Mistakes

- Aiming for 100% coverage blindly
- Testing implementation details for coverage
- Not covering edge cases
- Ignoring branch coverage
- Using coverage as only quality metric

## Related Topics

- [[What-is-Jest]]
- [[Write-UnitTests]]
- [[Run-Tests]]

## Quick Revision

| Metric | Measures |
|--------|----------|
| Statement | Code statements executed |
| Branch | If/else paths taken |
| Function | Functions called |
| Line | Lines executed |
| Threshold | Minimum coverage required |
