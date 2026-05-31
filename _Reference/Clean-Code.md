# Clean Code

## Definition

**Clean code** is code that is easy to read, understand, and maintain. It follows consistent conventions, communicates intent clearly, and minimizes complexity. Writing clean code is essential for team collaboration and long-term project success.

## Naming Conventions

### Variables and Functions

```javascript
// Bad
const x = 25;
const d = new Date();
const arr = users.filter((u) => u.a > 18);

// Good
const age = 25;
const currentDate = new Date();
const adultUsers = users.filter((user) => user.age > 18);
```

### Booleans

```javascript
// Bad
const active = true;
const check = false;

// Good
const isActive = true;
const hasPermission = false;
const canEdit = true;
```

### Functions

```javascript
// Bad
function process(data) { ... }
function handle() { ... }

// Good
function calculateTotalPrice(items) { ... }
function formatCurrency(amount) { ... }
function fetchUserData(userId) { ... }
```

## Functions

### Keep Functions Small and Focused

```javascript
// Bad - Does too much
function createAndSendEmail(user) {
  const template = generateTemplate(user);
  const html = compile(template);
  const mailOptions = {
    to: user.email,
    subject: "Welcome!",
    html: html,
  };
  transporter.sendMail(mailOptions);
}

// Good - Single responsibility
function generateWelcomeEmail(user) {
  const template = generateTemplate(user);
  return compile(template);
}

function sendEmail(to, subject, html) {
  return transporter.sendMail({ to, subject, html });
}
```

### Avoid Deep Nesting

```javascript
// Bad
function processOrder(order) {
  if (order) {
    if (order.items) {
      if (order.items.length > 0) {
        if (order.payment) {
          // process...
        }
      }
    }
  }
}

// Good - Use early returns (Guard clauses)
function processOrder(order) {
  if (!order) return;
  if (!order.items || order.items.length === 0) return;
  if (!order.payment) return;

  // process...
}
```

## Comments

### Prefer Self-Documenting Code

```javascript
// Bad
// Check if user is eligible for discount
if (user.age > 65 || user.isStudent) { ... }

// Good - Code is self-explanatory
const isEligibleForDiscount = user.age > 65 || user.isStudent;
if (isEligibleForDiscount) { ... }
```

### When to Comment

```javascript
// Good - Explains WHY, not WHAT
// We skip Sunday because the API returns stale data on that day
const validDays = ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat"];

// Good - Complex algorithms
// Using Boyer-Moore majority vote algorithm for finding the majority element
function findMajorityElement(nums) { ... }

// Bad - Unnecessary comment
// Increment counter by 1
counter++;
```

## Code Organization

### Group Related Code

```javascript
// Organize by feature/concern
class UserService {
  // --- User CRUD ---
  createUser(data) { ... }
  updateUser(id, data) { ... }
  deleteUser(id) { ... }

  // --- Authentication ---
  login(credentials) { ... }
  logout() { ... }

  // --- Permissions ---
  checkAccess(userId, resource) { ... }
  grantPermission(userId, permission) { ... }
}
```

### Use DRY (Don't Repeat Yourself)

```javascript
// Bad
const usdFormatter = (amount) => `$${amount.toFixed(2)}`;
const eurFormatter = (amount) => `€${amount.toFixed(2)}`;

// Good
function createCurrencyFormatter(symbol) {
  return (amount) => `${symbol}${amount.toFixed(2)}`;
}

const usdFormatter = createCurrencyFormatter("$");
const eurFormatter = createCurrencyFormatter("€");
```

## Error Handling

```javascript
// Bad
function divide(a, b) {
  return a / b; // Silently returns Infinity or NaN
}

// Good
function divide(a, b) {
  if (typeof a !== "number" || typeof b !== "number") {
    throw new TypeError("Arguments must be numbers");
  }
  if (b === 0) {
    throw new Error("Cannot divide by zero");
  }
  return a / b;
}
```

## Conditionals

### Prefer Positive Conditions

```javascript
// Bad
if (!isLoggedIn) { ... }

// Good (when possible)
if (isLoggedIn) {
  // show dashboard
} else {
  // show login
}
```

### Use Meaningful Conditions

```javascript
// Bad
if (age >= 18) { ... }

// Good
const isAdult = age >= 18;
if (isAdult) { ... }
```

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Magic numbers/strings | Extract to named constants |
| Long parameter lists | Use objects or builder pattern |
| Inconsistent naming | Establish and follow naming conventions |
| God objects/classes | Apply Single Responsibility Principle |
| Premature optimization | Write clear code first, optimize later |

## Quick Revision

- Use descriptive, meaningful names
- Keep functions small and focused (single responsibility)
- Use early returns to reduce nesting
- Write comments that explain WHY, not WHAT
- Follow DRY principle
- Handle errors explicitly
- Group related code together
- Prefer readability over cleverness

## Related Topics

- [[Design-Patterns]]
- [[Refactoring]]
- [[SOLID-Principles]]
- [[Testing]]
- [[Code-Review]]
