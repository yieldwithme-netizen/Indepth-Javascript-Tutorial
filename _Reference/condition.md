# Conditions

## Definition

**Conditions** allow code execution based on whether expressions evaluate to `true` or `false`. They are the foundation of decision-making in programming, controlling program flow through if/else statements, switch cases, and ternary operators.

## If Statement

```javascript
const temperature = 25;

if (temperature > 30) {
  console.log("It's hot outside!");
}
```

## If-Else Statement

```javascript
const age = 16;

if (age >= 18) {
  console.log("You can vote");
} else {
  console.log("You cannot vote yet");
}
```

## Else-If Ladder

```javascript
const score = 85;

if (score >= 90) {
  console.log("Grade: A");
} else if (score >= 80) {
  console.log("Grade: B");
} else if (score >= 70) {
  console.log("Grade: C");
} else if (score >= 60) {
  console.log("Grade: D");
} else {
  console.log("Grade: F");
}
```

## Nested Conditions

```javascript
const isLoggedIn = true;
const isAdmin = true;

if (isLoggedIn) {
  if (isAdmin) {
    console.log("Welcome, Admin");
  } else {
    console.log("Welcome, User");
  }
} else {
  console.log("Please log in");
}
```

### Guard Clauses (Early Returns)

Prefer flat nesting over deep nesting:

```javascript
// Bad - Deep nesting
function processOrder(order) {
  if (order) {
    if (order.items) {
      if (order.items.length > 0) {
        if (order.payment) {
          return "Processing...";
        }
      }
    }
  }
  return "Invalid order";
}

// Good - Guard clauses
function processOrder(order) {
  if (!order) return "Invalid order";
  if (!order.items || order.items.length === 0) return "No items";
  if (!order.payment) return "No payment";

  return "Processing...";
}
```

## Ternary Operator

Concise conditional expressions:

```javascript
// Syntax: condition ? valueIfTrue : valueIfFalse

const age = 20;
const status = age >= 18 ? "Adult" : "Minor";
console.log(status); // "Adult"

// Nested ternary (use sparingly)
const score = 75;
const grade =
  score >= 90 ? "A" :
  score >= 80 ? "B" :
  score >= 70 ? "C" : "F";
console.log(grade); // "C"

// With function calls
function greet(isMorning) {
  return isMorning ? getMorningGreeting() : getEveningGreeting();
}
```

## Switch Statement

Best for multiple conditions against a single value:

```javascript
const day = "Monday";

switch (day) {
  case "Monday":
    console.log("Start of work week");
    break;
  case "Friday":
    console.log("TGIF!");
    break;
  case "Saturday":
  case "Sunday":
    console.log("Weekend!");
    break;
  default:
    console.log("Midweek");
}
```

### Switch with Expressions

```javascript
const getDiscount = (tier) => {
  switch (tier) {
    case "gold":
      return 30;
    case "silver":
      return 20;
    case "bronze":
      return 10;
    default:
      return 0;
  }
};
```

## Short-Circuit Evaluation

```javascript
// AND (&&) - returns first falsy or last value
const name = "Alice";
const greeting = name && `Hello, ${name}`; // "Hello, Alice"

const empty = "";
const result = empty && "Hello"; // "" (empty string is falsy)

// OR (||) - returns first truthy or last value
const username = "" || "Anonymous"; // "Anonymous"
const username2 = "Bob" || "Anonymous"; // "Bob"

// Nullish coalescing (??) - only null/undefined trigger default
const count = 0;
const display1 = count || "N/A";   // "N/A" (0 is falsy)
const display2 = count ?? "N/A";   // 0 (not null/undefined)
```

## Pattern Matching

```javascript
function classifyUser(user) {
  if (user?.isAdmin) return "Administrator";
  if (user?.role === "moderator") return "Moderator";
  if (user?.verified) return "Verified User";
  return "Guest";
}
```

## Truthiness Checks

```javascript
// Check for existence
const items = [1, 2, 3];
if (items.length) {
  console.log("Has items");
}

// Check for value
const config = null;
const settings = config ?? getDefaultSettings();

// Boolean conversion
function toBoolean(value) {
  return !!value;
}
```

## Common Use Cases

```javascript
// Feature flags
if (featureFlags.darkMode) {
  document.body.classList.add("dark");
}

// Role-based access
const canEdit = user.role === "admin" || user.role === "editor";

// Validation
const isValid = email && email.includes("@") && email.length > 3;

// Conditional rendering (React)
{isLoggedIn ? <Dashboard /> : <LoginPage />}
```

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Using `=` in conditions | Use `===` for comparison |
| Missing `break` in switch | Always add `break` or use `return` |
| Comparing with `NaN` | Use `Number.isNaN()` |
| Deeply nested ifs | Use guard clauses |
| Confusing `&&` and `\|\|` precedence | Add parentheses for clarity |

## Quick Revision

- **if/else** for basic branching
- **else-if** for multiple exclusive branches
- **switch** for matching a single value against many
- **Ternary** (`? :`) for concise conditional assignments
- **Guard clauses** reduce nesting (early returns)
- **Short-circuit** (`&&`, `||`, `??`) for concise logic
- Always use `===` for comparisons

## Related Topics

- [[Comparison-Operators]]
- [[Truthy-Falsy]]
- [[Ternary-Operator]]
- [[Logical-Operators]]
- [[Switch-Statement]]
- [[Nullish-Coalescing]]
