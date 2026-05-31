# What is Testing?

## Definition

Testing is the process of verifying that software works as expected. It involves writing automated tests that check code behavior, catch bugs early, enable safe refactoring, and serve as documentation.

---

## Types of Tests

### Unit Tests

Test individual functions or components in isolation.

```javascript
// Function to test
function add(a, b) {
  return a + b;
}

// Unit test
describe("add", () => {
  it("should add two numbers", () => {
    expect(add(1, 2)).toBe(3);
  });

  it("should handle negative numbers", () => {
    expect(add(-1, -2)).toBe(-3);
  });

  it("should handle zero", () => {
    expect(add(0, 5)).toBe(5);
  });
});
```

### Integration Tests

Test how multiple units work together.

```javascript
// Integration test
describe("User API", () => {
  it("should create and retrieve a user", async () => {
    const user = await createUser({ name: "Alice", email: "alice@test.com" });
    const retrieved = await getUser(user.id);

    expect(retrieved.name).toBe("Alice");
    expect(retrieved.email).toBe("alice@test.com");
  });
});
```

### End-to-End (E2E) Tests

Test the entire application from the user's perspective.

```javascript
// E2E test with Playwright
test("user can login", async ({ page }) => {
  await page.goto("/login");
  await page.fill('[data-testid="email"]', "user@test.com");
  await page.fill('[data-testid="password"]', "password123");
  await page.click('[data-testid="submit"]');

  await expect(page).toHaveURL("/dashboard");
  await expect(page.locator("h1")).toContainText("Welcome");
});
```

---

## Testing Frameworks

### Jest

```javascript
// sum.test.js
function sum(a, b) {
  return a + b;
}

describe("sum", () => {
  test("adds 1 + 2 to equal 3", () => {
    expect(sum(1, 2)).toBe(3);
  });

  test("adds negative numbers", () => {
    expect(sum(-1, -2)).toBe(-3);
  });
});
```

### Vitest

```javascript
// sum.test.js
import { describe, it, expect } from "vitest";
import { sum } from "./sum";

describe("sum", () => {
  it("adds 1 + 2 to equal 3", () => {
    expect(sum(1, 2)).toBe(3);
  });
});
```

### Mocha + Chai

```javascript
const { expect } = require("chai");

describe("Array", () => {
  describe("#indexOf()", () => {
    it("should return -1 when the value is not present", () => {
      expect([1, 2, 3].indexOf(4)).to.equal(-1);
    });
  });
});
```

---

## Test Patterns

### Arrange-Act-Assert (AAA)

```javascript
test("calculate discount", () => {
  // Arrange
  const price = 100;
  const discountPercent = 20;

  // Act
  const result = calculateDiscount(price, discountPercent);

  // Assert
  expect(result).toBe(80);
});
```

### Given-When-Then (BDD)

```javascript
describe("Shopping Cart", () => {
  it("should apply discount when total exceeds 100", () => {
    // Given
    const cart = new ShoppingCart();
    cart.addItem({ price: 60 });
    cart.addItem({ price: 60 });

    // When
    cart.applyDiscount();

    // Then
    expect(cart.total).toBe(108); // 120 - 10%
  });
});
```

### Mocking

```javascript
// Mock function
const mockFn = jest.fn();
mockFn.mockReturnValue(42);

expect(mockFn()).toBe(42);
expect(mockFn).toHaveBeenCalledTimes(1);

// Mock module
jest.mock("./database");
import { db } from "./database";
db.users.findMany.mockResolvedValue([{ id: 1, name: "Alice" }]);

// Mock API calls
global.fetch = jest.fn(() =>
  Promise.resolve({
    ok: true,
    json: () => Promise.resolve({ name: "Alice" })
  })
);
```

### Spying

```javascript
const obj = {
  method: () => "original"
};

const spy = jest.spyOn(obj, "method");
spy.mockReturnValue("mocked");

expect(obj.method()).toBe("mocked");
expect(spy).toHaveBeenCalled();

spy.mockRestore(); // Restore original
```

---

## Common Use Cases

### Testing Pure Functions

```javascript
// Pure function - easy to test
function factorial(n) {
  if (n < 0) throw new Error("Negative not allowed");
  if (n === 0) return 1;
  return n * factorial(n - 1);
}

describe("factorial", () => {
  it("returns 1 for 0", () => {
    expect(factorial(0)).toBe(1);
  });

  it("returns 120 for 5", () => {
    expect(factorial(5)).toBe(120);
  });

  it("throws for negative numbers", () => {
    expect(() => factorial(-1)).toThrow("Negative not allowed");
  });
});
```

### Testing Async Code

```javascript
// Async function
async function fetchUser(id) {
  const response = await fetch(`/api/users/${id}`);
  if (!response.ok) throw new Error("User not found");
  return response.json();
}

describe("fetchUser", () => {
  beforeEach(() => {
    global.fetch = jest.fn();
  });

  it("returns user data", async () => {
    const mockUser = { id: 1, name: "Alice" };
    global.fetch.mockResolvedValue({
      ok: true,
      json: () => Promise.resolve(mockUser)
    });

    const user = await fetchUser(1);

    expect(user).toEqual(mockUser);
    expect(global.fetch).toHaveBeenCalledWith("/api/users/1");
  });

  it("throws on error", async () => {
    global.fetch.mockResolvedValue({ ok: false });

    await expect(fetchUser(1)).rejects.toThrow("User not found");
  });
});
```

### Testing React Components

```javascript
import { render, screen, fireEvent } from "@testing-library/react";

function Counter() {
  const [count, setCount] = React.useState(0);
  return (
    <div>
      <span data-testid="count">{count}</span>
      <button onClick={() => setCount(c => c + 1)}>Increment</button>
    </div>
  );
}

describe("Counter", () => {
  it("increments on click", () => {
    render(<Counter />);

    expect(screen.getByTestId("count")).toHaveTextContent("0");

    fireEvent.click(screen.getByText("Increment"));

    expect(screen.getByTestId("count")).toHaveTextContent("1");
  });
});
```

### Test Coverage

```javascript
// jest.config.js
module.exports = {
  collectCoverageFrom: [
    "src/**/*.{js,jsx}",
    "!src/index.js"
  ],
  coverageThreshold: {
    global: {
      branches: 80,
      functions: 80,
      lines: 80,
      statements: 80
    }
  }
};

// Run with coverage
// jest --coverage
```

---

## Common Mistakes

### Mistake 1: Testing Implementation Details

```javascript
// Wrong: tests internal state
test("component uses useState", () => {
  const { container } = render(<Counter />);
  expect(container._reactInternals).toBeDefined();
});

// Correct: test behavior
test("increments count on click", () => {
  render(<Counter />);
  fireEvent.click(screen.getByText("Increment"));
  expect(screen.getByTestId("count")).toHaveTextContent("1");
});
```

### Mistake 2: Not Isolating Tests

```javascript
// Wrong: tests depend on each other
let counter = 0;

test("increments", () => {
  counter++;
  expect(counter).toBe(1);
});

test("increments again", () => {
  counter++;
  expect(counter).toBe(2); // Fails if run in isolation!
});

// Correct: reset between tests
beforeEach(() => {
  counter = 0;
});
```

### Mistake 3: Over-Mocking

```javascript
// Wrong: mocking everything
jest.mock("./utils");
jest.mock("./api");
jest.mock("./config");

// Correct: only mock what's necessary
jest.mock("./api"); // External dependency
// Let utils and config be real
```

### Mistake 4: Ignoring Edge Cases

```javascript
// Wrong: only testing happy path
test("adds numbers", () => {
  expect(add(1, 2)).toBe(3);
});

// Correct: test edge cases
test("adds numbers", () => {
  expect(add(1, 2)).toBe(3);      // Normal
  expect(add(-1, -2)).toBe(-3);    // Negative
  expect(add(0, 0)).toBe(0);       // Zero
  expect(add(0.1, 0.2)).toBeCloseTo(0.3); // Floating point
});
```

---

## Quick Revision Summary

| Test Type | Scope | Speed | Cost |
|-----------|-------|-------|------|
| Unit | Single function | Fast | Low |
| Integration | Multiple units | Medium | Medium |
| E2E | Entire app | Slow | High |

| Framework | Use Case |
|-----------|----------|
| Jest | General purpose |
| Vitest | Vite projects |
| Mocha | Flexible setup |
| Cypress | E2E browser testing |
| Playwright | Cross-browser E2E |

---

## Related Topics

- [[debugging]] - Finding and fixing bugs
- [[Promise]] - Testing async code
- [[API-Design]] - API testing strategies
- [[Object]] - Mocking objects
- [[Array]] - Testing array operations
