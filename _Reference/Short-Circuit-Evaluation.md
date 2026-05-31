# Short-Circuit Evaluation

## Definition

Short-circuit evaluation is a behavior in JavaScript where the second argument is executed or evaluated only if the first argument does not determine the result. This applies to logical operators `&&` (AND) and `||` (OR).

## How It Works

### AND (`&&`) Operator

Returns the first falsy value, or the last value if all are truthy.

```javascript
// Returns first falsy value
console.log(false && anything); // false
console.log(null && anything);  // null
console.log(undefined && anything); // undefined
console.log(0 && anything);     // 0
console.log('' && anything);    // ""

// Returns last value if all truthy
console.log(true && 'hello');   // "hello"
console.log('a' && 'b');        // "b"
console.log(1 && 2);            // 2
```

### OR (`||`) Operator

Returns the first truthy value, or the last value if all are falsy.

```javascript
// Returns first truthy value
console.log(true || anything);  // true
console.log('hello' || anything); // "hello"
console.log(1 || anything);     // 1

// Returns last value if all falsy
console.log(false || null);     // null
console.log(false || 0 || '');  // ""
console.log(false || undefined || null); // null
```

## Practical Examples

### Default Values

```javascript
// ❌ Verbose approach
function greet(name) {
  if (name === undefined || name === null) {
    name = 'Guest';
  }
  return `Hello, ${name}!`;
}

// ✅ Short-circuit approach
function greet(name) {
  name = name || 'Guest';
  return `Hello, ${name}!`;
}

// ✅ Modern: Nullish coalescing (only null/undefined)
function greet(name) {
  name = name ?? 'Guest';
  return `Hello, ${name}!`;
}
```

### Conditional Execution

```javascript
// Execute function only if condition is true
const user = { name: 'John', isAdmin: true };

// ❌ Verbose
if (user.isAdmin) {
  adminAction();
}

// ✅ Short-circuit
user.isAdmin && adminAction();

// Common in React JSX
{isLoggedIn && <UserProfile />}
```

### Property Access

```javascript
// Safe property access
const user = { profile: { name: 'Alice' } };

// ❌ May throw error
// const city = user.address.city;

// ✅ Short-circuit
const city = user.address && user.address.city;

// ✅ Modern: Optional chaining
const city = user.address?.city;
```

### Function Arguments

```javascript
// Only call if value exists
function processData(data) {
  // ❌ Verbose
  if (data !== null && data !== undefined) {
    process(data);
  }

  // ✅ Short-circuit
  data != null && process(data);
}
```

## Comparison: `&&` vs `||`

```javascript
// AND: Returns first falsy OR last value
console.log(1 && 2 && 3);     // 3
console.log(1 && null && 3);  // null
console.log(null && 2 && 3);  // null

// OR: Returns first truthy OR last value
console.log(1 || 2 || 3);     // 1
console.log(null || 2 || 3);  // 2
console.log(null || undefined || 3); // 3
```

## Common Use Cases

### Configuration Defaults

```javascript
// Merge with defaults
const config = {
  timeout: userTimeout || 5000,
  retries: userRetries || 3,
  debug: userDebug || false
};
```

### Conditional Rendering (React)

```javascript
function App({ user, notifications }) {
  return (
    <div>
      {user && <Header user={user} />}
      {notifications.length > 0 && <NotificationList items={notifications} />}
    </div>
  );
}
```

### Guard Clauses

```javascript
function processPayment(user, amount) {
  return user && user.isLoggedIn && amount > 0 && 
    chargeCard(user.card, amount);
}
```

## Nullish Coalescing (`??`)

Modern alternative to `||` that only checks for `null` or `undefined`.

```javascript
// || treats 0, '', false as falsy
const count = 0 || 10;  // 10 (wrong for some cases)
const name = '' || 'Anonymous'; // "Anonymous"

// ?? only treats null/undefined as nullish
const count = 0 ?? 10;  // 0 (correct)
const name = '' ?? 'Anonymous'; // "" (preserves empty string)
```

## Common Mistakes

```javascript
// ❌ Wrong: Unexpected falsy values
const value = 0 || 'default'; // "default" (0 is falsy)
const items = [] || ['default']; // ['default'] (empty array is truthy)

// ✅ Better: Use ?? for intentional defaults
const value = 0 ?? 'default'; // 0

// ❌ Wrong: Complex conditions become unreadable
const result = a && b && c && d && e || f && g;

// ✅ Better: Explicit condition
const result = (a && b && c && d && e) || (f && g);

// ❌ Wrong: Assignment with && 
// x && x = 5; // SyntaxError

// ✅ Correct: Use || for assignment
x = x || 5;
```

## Readability Tips

```javascript
// ❌ Hard to read
const name = user && user.profile && user.profile.name || 'Anonymous';

// ✅ Better with optional chaining
const name = user?.profile?.name ?? 'Anonymous';

// ✅ Break into steps for complex logic
const hasAccess = user && user.isLoggedIn && user.role === 'admin';
hasAccess && grantAccess();
```

## Related Topics

- [[Logical-Operators]] - AND, OR, NOT operators
- [[Nullish-Coalescing]] - The `??` operator
- [[Optional-Chaining]] - The `?.` operator
- [[Truthy-Falsy]] - Values that evaluate to true/false
- [[Ternary-Operator]] - Conditional expressions
- [[Conditional-Logic]] - Decision-making in code

## Quick Revision

| Operator | Returns | Use Case |
|----------|---------|----------|
| `&&` | First falsy or last value | Conditional execution |
| `\|\|` | First truthy or last value | Default values |
| `??` | First non-null/undefined | Precise defaults |

**Key Rules:**
- `&&` short-circuits on falsy
- `||` short-circuits on truthy
- `??` only short-circuits on `null`/`undefined`
- Return values are the actual values, not just `true`/`false`

**Best Practices:**
- Use `??` instead of `||` for numeric/string defaults
- Keep expressions readable
- Use for guard clauses and defaults
- Avoid complex nested short-circuits