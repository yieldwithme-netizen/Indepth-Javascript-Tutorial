# Code Comments

## Definition
Code comments are non-executable text annotations in source code that explain functionality, intent, and usage. They help developers understand code purpose without reading implementation details.

## Types of Comments

### Single-Line Comments
```javascript
// This is a single-line comment
const x = 10; // Inline comment

// Calculate the total price
const total = price + tax;
```

### Multi-Line Comments
```javascript
/*
  This function calculates the
  total price including tax
  and applies any discounts.
*/
function calculateTotal(price, taxRate, discount) {
  return (price * (1 + taxRate)) - discount;
}
```

### JSDoc Comments (Documentation)
```javascript
/**
 * Calculates the total price including tax and discount.
 * @param {number} price - The base price of the item
 * @param {number} taxRate - The tax rate as a decimal (e.g., 0.08 for 8%)
 * @param {number} discount - The discount amount
 * @returns {number} The final total price
 * @throws {Error} If price is negative
 * @example
 * calculateTotal(100, 0.08, 10) // returns 98
 */
function calculateTotal(price, taxRate, discount) {
  if (price < 0) throw new Error('Price cannot be negative');
  return (price * (1 + taxRate)) - discount;
}
```

### TODO Comments
```javascript
// TODO: Add input validation
// FIXME: This breaks with negative numbers
// HACK: Temporary workaround until API is fixed
// NOTE: This requires Node.js 18+
// XXX: Do not use in production
```

### Commented-Out Code (Avoid)
```javascript
// BAD: Don't do this
// const oldResult = data.filter(item => item.active);
// const sorted = oldResult.sort((a, b) => a.name - b.name);
// return sorted;
```

## When to Comment

### Good Comments
```javascript
// Convert price from cents to dollars for display
const displayPrice = priceInCents / 100;

// Retry logic: API sometimes returns 503 during peak hours
const MAX_RETRIES = 3;

/*
 * Algorithm explanation:
 * Uses binary search to find the target value in O(log n) time.
 * The array must be sorted for this to work correctly.
 */
function binarySearch(arr, target) {
  let left = 0;
  let right = arr.length - 1;
  while (left <= right) {
    const mid = Math.floor((left + right) / 2);
    if (arr[mid] === target) return mid;
    if (arr[mid] < target) left = mid + 1;
    else right = mid - 1;
  }
  return -1;
}
```

### Bad Comments (Avoid)
```javascript
// BAD: States the obvious
const users = []; // array of users

// BAD: Outdated comment
// This function was updated in v2.0
function process() { /* ... */ }

// BAD: Instead, rename the variable
const d = new Date(); // BAD
const currentDate = new Date(); // GOOD - no comment needed
```

## Common Use Cases
- Documenting function parameters and return values
- Explaining complex algorithms or business logic
- Marking workarounds and known issues
- Providing usage examples via JSDoc
- Annotating regulatory or compliance requirements

## Common Mistakes
- Commenting obvious code instead of improving naming
- Leaving stale/outdated comments that mislead
- Using comments to hide bad code instead of refactoring
- Over-commenting every line
- Writing comments in wrong language or style

## Related Topics
- [[Code-Style]]
- [[JSDoc]]
- [[Naming-Conventions]]
- [[Refactoring]]
- [[IDE-Features]]

## Quick Revision
- Use `//` for single-line, `/* */` for multi-line
- JSDoc (`/** */`) generates documentation automatically
- Comment **why**, not **what** — code shows what it does
- Avoid commenting out code; use version control instead
- Keep comments up to date or remove them
- TODO/FIXME markers track technical debt
