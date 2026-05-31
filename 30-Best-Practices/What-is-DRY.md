# What is DRY Principle?

## Definition

DRY stands for **Don't Repeat Yourself** — a principle stating that every piece of knowledge should have a single, unambiguous representation in a system.

## DRY vs WET

| Principle | Meaning | Example |
|-----------|---------|---------|
| **DRY** | Don't Repeat Yourself | Reuse code |
| **WET** | Write Everything Twice | Duplicate code |

## Repetition Examples

```javascript
// BAD: Repeated logic
function calculateArea(width, height) {
  if (width <= 0 || height <= 0) {
    throw new Error("Dimensions must be positive");
  }
  return width * height;
}

function calculatePerimeter(width, height) {
  if (width <= 0 || height <= 0) {
    throw new Error("Dimensions must be positive");
  }
  return 2 * (width + height);
}

// GOOD: Extract validation
function validateDimensions(width, height) {
  if (width <= 0 || height <= 0) {
    throw new Error("Dimensions must be positive");
  }
}

function calculateArea(width, height) {
  validateDimensions(width, height);
  return width * height;
}

function calculatePerimeter(width, height) {
  validateDimensions(width, height);
  return 2 * (width + height);
}
```

## Functions

```javascript
// BAD: Repeated email validation
function validateEmail(email) {
  return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
}

function createUser(email) {
  if (!validateEmail(email)) {
    throw new Error("Invalid email");
  }
  // Create user
}

function updateUser(email) {
  if (!validateEmail(email)) {
    throw new Error("Invalid email");
  }
  // Update user
}

// GOOD: Single validation function
function validateEmail(email) {
  if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email)) {
    throw new Error("Invalid email");
  }
}

function createUser(email) {
  validateEmail(email);
  // Create user
}

function updateUser(email) {
  validateEmail(email);
  // Update user
}
```

## Objects and Classes

```javascript
// BAD: Duplicate object structure
const user1 = {
  name: "John",
  email: "john@example.com",
  validate() {
    if (!this.name) throw new Error("Name required");
    if (!this.email) throw new Error("Email required");
  },
};

const user2 = {
  name: "Jane",
  email: "jane@example.com",
  validate() {
    if (!this.name) throw new Error("Name required");
    if (!this.email) throw new Error("Email required");
  },
};

// GOOD: Use a class
class User {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  validate() {
    if (!this.name) throw new Error("Name required");
    if (!this.email) throw new Error("Email required");
  }
}

const user1 = new User("John", "john@example.com");
const user2 = new User("Jane", "jane@example.com");
```

## Constants and Configuration

```javascript
// BAD: Magic numbers
function calculateDiscount(price) {
  if (price > 1000) return price * 0.2;
  if (price > 500) return price * 0.15;
  return price * 0.1;
}

// GOOD: Named constants
const DISCOUNT_TIERS = {
  LARGE: { min: 1000, rate: 0.2 },
  MEDIUM: { min: 500, rate: 0.15 },
  SMALL: { min: 0, rate: 0.1 },
};

function calculateDiscount(price) {
  if (price > DISCOUNT_TIERS.LARGE.min) return price * DISCOUNT_TIERS.LARGE.rate;
  if (price > DISCOUNT_TIERS.MEDIUM.min) return price * DISCOUNT_TIERS.MEDIUM.rate;
  return price * DISCOUNT_TIERS.SMALL.rate;
}
```

## Template Literals

```javascript
// BAD: Repeated string construction
const greeting1 = "Hello, " + firstName + " " + lastName + "!";
const greeting2 = "Goodbye, " + firstName + " " + lastName + "!";

// GOOD: Function for repeated patterns
function formatName(first, last) {
  return `${first} ${last}`;
}

const greeting1 = `Hello, ${formatName(firstName, lastName)}!`;
const greeting2 = `Goodbye, ${formatName(firstName, lastName)}!`;
```

## When NOT to Be DRY

```javascript
// BAD: Over-abstracting simple code
const add = (a, b) => a + b;
const sum = (a, b) => a + b; // Same thing, don't merge

// BAD: Premature abstraction
function processUser(user) {
  validateUser(user);
  saveUser(user);
  sendWelcomeEmail(user);
  logActivity(user, "created");
}

// Later: requirements diverge
// processAdmin and processGuest need different steps
// Now you have to undo the abstraction

// GOOD: Keep it simple until patterns emerge
function processUser(user) {
  validateUser(user);
  saveUser(user);
  sendWelcomeEmail(user);
  logActivity(user, "created");
}

// Wait for a second use case before abstracting
```

## Common Mistakes

```javascript
// BAD: Duplicating logic across files
// utils/validation.js
export function validateEmail(email) { ... }

// services/user.js
function validateEmail(email) { ... } // Duplicate!

// GOOD: Import from single source
import { validateEmail } from "../utils/validation.js";

// BAD: Copy-pasting code
function processOrder1(order) { ... }
function processOrder2(order) { ... } // Mostly same

// GOOD: Parameterize differences
function processOrder(order, type) {
  // Common logic
  if (type === "express") {
    // Express-specific logic
  }
}
```

## Quick Revision

- DRY = Don't Repeat Yourself
- Extract repeated logic into functions
- Use classes for repeated object structures
- Define constants for magic numbers
- Don't over-abstract; wait for patterns
- Import shared utilities instead of duplicating

---

## Related Topics

- [[What-is-CleanCode]] - Clean code principles
- [[What-is-SOLID]] - SOLID principles
- [[What-is-CodeReview]] - Code review
- [[What-is-Documentation]] - Documentation
- [[Debug-JavaScript]] - Debugging