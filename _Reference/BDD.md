# BDD (Behavior-Driven Development)

## Definition
BDD (Behavior-Driven Development) is a software development methodology that extends TDD by writing test cases in natural language. It focuses on the behavior of the application from the user's perspective using Given-When-Then syntax.

## Common BDD Frameworks
- Cucumber
- Jasmine
- Jest (with BDD style)
- Mocha + Chai

## Code Examples

### Jasmine BDD Syntax
```javascript
describe('Calculator', function() {
  let calculator;

  beforeEach(function() {
    calculator = new Calculator();
  });

  describe('addition', function() {
    it('should add two numbers correctly', function() {
      expect(calculator.add(2, 3)).toBe(5);
    });

    it('should handle negative numbers', function() {
      expect(calculator.add(-1, -2)).toBe(-3);
    });
  });

  describe('subtraction', function() {
    it('should subtract two numbers correctly', function() {
      expect(calculator.subtract(5, 3)).toBe(2);
    });
  });
});
```

### Jest BDD Style
```javascript
describe('Shopping Cart', () => {
  let cart;

  beforeEach(() => {
    cart = new ShoppingCart();
  });

  describe('when adding items', () => {
    it('should increase item count', () => {
      cart.addItem({ id: 1, name: 'Product A', price: 10 });
      expect(cart.itemCount).toBe(1);
    });

    it('should calculate total correctly', () => {
      cart.addItem({ id: 1, name: 'Product A', price: 10 });
      cart.addItem({ id: 2, name: 'Product B', price: 20 });
      expect(cart.total).toBe(30);
    });
  });

  describe('when removing items', () => {
    it('should decrease item count', () => {
      cart.addItem({ id: 1, name: 'Product A', price: 10 });
      cart.removeItem(1);
      expect(cart.itemCount).toBe(0);
    });
  });
});
```

### Cucumber/Gherkin Syntax
```gherkin
Feature: User Login

  Scenario: Successful login with valid credentials
    Given I am on the login page
    When I enter valid credentials
    And I click the login button
    Then I should be redirected to the dashboard
    And I should see a welcome message

  Scenario: Failed login with invalid password
    Given I am on the login page
    When I enter invalid password
    And I click the login button
    Then I should see an error message
    And I should remain on the login page
```

### Custom BDD Framework
```javascript
class BDD {
  constructor() {
    this.tests = [];
    this.currentDescribe = '';
  }

  describe(name, fn) {
    this.currentDescribe = name;
    fn();
  }

  it(name, fn) {
    this.tests.push({
      describe: this.currentDescribe,
      name,
      fn
    });
  }

  run() {
    this.tests.forEach(test => {
      try {
        test.fn();
        console.log(`✓ ${test.describe} > ${test.name}`);
      } catch (error) {
        console.log(`✗ ${test.describe} > ${test.name}`);
        console.log(`  ${error.message}`);
      }
    });
  }
}

// Usage
const bdd = new BDD();

bdd.describe('Array', () => {
  bdd.it('should have length property', () => {
    const arr = [1, 2, 3];
    if (arr.length !== 3) throw new Error('Expected length 3');
  });

  bdd.it('should support push', () => {
    const arr = [];
    arr.push(1);
    if (arr.length !== 1) throw new Error('Expected length 1');
  });
});

bdd.run();
```

## Matchers/Expectations
```javascript
// Equality
expect(value).toBe(expected);
expect(value).toEqual(expected);

// Truthiness
expect(value).toBeTruthy();
expect(value).toBeFalsy();
expect(value).toBeNull();
expect(value).toBeUndefined();

// Numbers
expect(value).toBeGreaterThan(3);
expect(value).toBeLessThan(10);

// Strings
expect(string).toMatch(/regex/);

// Arrays
expect(array).toContain(item);
expect(array).toHaveLength(3);

// Exceptions
expect(fn).toThrow(Error);
```

## Common Use Cases
- Acceptance testing
- Integration testing
- Documentation as tests
- Team collaboration

## Common Mistakes
- **Over-specifying**: Keep scenarios focused on behavior
- **Testing implementation details**: Focus on what, not how
- **Unclear scenarios**: Write scenarios that anyone can understand
- **Missing setup/teardown**: Use `beforeEach`/`afterEach`

## Related Topics
- [[Unit-Testing]]
- [[Jest]]
- [[Mocha]]
- [[TDD]]
- [[Test-Driven-Development]]

## Quick Revision
- BDD focuses on application behavior from user perspective
- Uses Given-When-Then syntax for scenarios
- Describes features in natural language
- Popular frameworks: Jasmine, Jest, Cucumber
- Encourages collaboration between developers and stakeholders
