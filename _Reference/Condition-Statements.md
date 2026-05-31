# Condition Statements

## Definition
Condition statements are control flow structures that execute different blocks of code based on whether a specified condition evaluates to true or false. JavaScript provides `if`, `else if`, `else`, and `switch` statements.

## If Statement
```javascript
// Basic if
if (isLoggedIn) {
  showDashboard();
}

// if-else
if (score >= 60) {
  console.log('Passed');
} else {
  console.log('Failed');
}

// if-else if-else chain
if (score >= 90) {
  grade = 'A';
} else if (score >= 80) {
  grade = 'B';
} else if (score >= 70) {
  grade = 'C';
} else {
  grade = 'F';
}
```

## Guard Clauses (Early Returns)
```javascript
// Instead of deeply nested if-else
function processUser(user) {
  if (!user) return null;
  if (!user.isActive) return null;
  if (!user.hasPermission) return null;

  return {
    id: user.id,
    name: user.name,
    email: user.email
  };
}
```

## Nested Conditions
```javascript
if (user) {
  if (user.isAuthenticated) {
    if (user.hasRole('admin')) {
      showAdminPanel();
    } else {
      showUserDashboard();
    }
  } else {
    promptLogin();
  }
} else {
  showLandingPage();
}

// Refactored with guard clauses
function handleUser(user) {
  if (!user) return showLandingPage();
  if (!user.isAuthenticated) return promptLogin();

  if (user.hasRole('admin')) {
    return showAdminPanel();
  }
  showUserDashboard();
}
```

## Switch Statement
```javascript
// Basic switch
const command = 'start';

switch (command) {
  case 'start':
    console.log('Starting...');
    break;
  case 'stop':
    console.log('Stopping...');
    break;
  case 'pause':
    console.log('Pausing...');
    break;
  default:
    console.log('Unknown command');
}

// Fall-through (intentional)
switch (month) {
  case 'December':
  case 'January':
  case 'February':
    console.log('Winter');
    break;
  case 'March':
  case 'April':
  case 'May':
    console.log('Spring');
    break;
  // ... other seasons
}
```

## Switch vs Object Lookup
```javascript
// Switch approach
function getHttpStatus(code) {
  switch (code) {
    case 200: return 'OK';
    case 201: return 'Created';
    case 404: return 'Not Found';
    case 500: return 'Server Error';
    default: return 'Unknown';
  }
}

// Object lookup approach (often cleaner)
const httpStatus = {
  200: 'OK',
  201: 'Created',
  404: 'Not Found',
  500: 'Server Error'
};

function getHttpStatus(code) {
  return httpStatus[code] ?? 'Unknown';
}
```

## Asserting Types with Conditions
```javascript
function processValue(value) {
  if (typeof value === 'string') {
    return value.toUpperCase();
  }
  if (typeof value === 'number') {
    return value.toFixed(2);
  }
  if (Array.isArray(value)) {
    return value.join(', ');
  }
  return String(value);
}
```

## Common Use Cases
- Route handling and navigation
- Permission checks
- Form validation
- State management
- Event handling

## Common Mistakes
- Missing `break` in switch (unintended fall-through)
- Deep nesting instead of guard clauses
- Using assignment `=` in conditions
- Not considering falsy values like `0` and `''`
- Overcomplicating simple boolean checks

## Related Topics
- [[Conditional-Logic]]
- [[Logical-Operators]]
- [[Functions]]
- [[Loops]]
- [[Type-Coercion]]

## Quick Revision
- `if/else if/else` for most conditional logic
- Use guard clauses to reduce nesting
- `switch` for checking one value against many cases
- Object lookup often replaces switch for simple mappings
- Always include a `default` case in switch statements
