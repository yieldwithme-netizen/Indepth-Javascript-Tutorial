# Code Review Best Practices

## Definition

Code review is a systematic examination of source code by peers to find bugs, improve code quality, share knowledge, and ensure adherence to coding standards. It's a critical practice for maintaining reliable, maintainable JavaScript applications.

## Why Code Review Matters

- **Bug prevention**: Catches issues before production
- **Knowledge sharing**: Spreads understanding across team
- **Consistency**: Maintains uniform code style
- **Learning**: Junior developers learn from seniors
- **Documentation**: Creates context for future changes

## Code Review Checklist

### 1. Functionality

```javascript
// ❌ Review: Does this handle edge cases?
function divide(a, b) {
  return a / b;
}
// Issue: Division by zero not handled

// ✅ Improved
function divide(a, b) {
  if (b === 0) throw new Error('Cannot divide by zero');
  return a / b;
}
```

### 2. Readability

```javascript
// ❌ Review: Cryptic variable names
const x = arr.filter(i => i.t > 5 && i.a === 'y');

// ✅ Improved: Descriptive names
const activeItems = items.filter(
  item => item.quantity > 5 && item.status === 'active'
);
```

### 3. Error Handling

```javascript
// ❌ Review: Missing error handling
async function fetchUser(id) {
  const response = await fetch(`/api/users/${id}`);
  return response.json();
}

// ✅ Improved: Proper error handling
async function fetchUser(id) {
  try {
    const response = await fetch(`/api/users/${id}`);
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    return await response.json();
  } catch (error) {
    console.error('Failed to fetch user:', error);
    throw error;
  }
}
```

### 4. Security

```javascript
// ❌ Review: XSS vulnerability
function renderUserInput(input) {
  document.innerHTML = input;
}

// ✅ Improved: Sanitized output
function renderUserInput(input) {
  const div = document.createElement('div');
  div.textContent = input;
  document.appendChild(div);
}
```

### 5. Performance

```javascript
// ❌ Review: Inefficient loop
const results = [];
for (let i = 0; i < arr.length; i++) {
  if (arr[i] > 10) {
    results.push(arr[i] * 2);
  }
}

// ✅ Improved: Method chaining
const results = arr
  .filter(item => item > 10)
  .map(item => item * 2);
```

## Common Review Points

### Naming Conventions

```javascript
// ❌ Bad naming
const d = new Date();
const fn = () => {};
const arr = [1, 2, 3];

// ✅ Good naming
const currentDate = new Date();
const handleUserClick = () => {};
const userScores = [1, 2, 3];
```

### Code Structure

```javascript
// ❌ Review: Long function, multiple responsibilities
function processUser(user) {
  // 50+ lines of validation, transformation, API calls...
}

// ✅ Improved: Single responsibility
function validateUser(user) { /* ... */ }
function formatUserData(user) { /* ... */ }
function saveUser(user) { /* ... */ }
```

### Comments and Documentation

```javascript
// ❌ Review: Unnecessary comment
// Increment counter
counter++;

// ✅ Better: Meaningful comment when needed
// Account for zero-indexed array offset
counter++;

// ✅ Best: Self-documenting code
arrayIndex++;
```

## Review Guidelines

### For Reviewers

1. **Be constructive**: Suggest improvements, don't just criticize
2. **Ask questions**: "Have you considered...?"
3. **Explain why**: Don't just say "change this"
4. **Prioritize**: Focus on critical issues first
5. **Praise good work**: Acknowledge improvements

### For Authors

1. **Self-review first**: Check your own code before submitting
2. **Provide context**: Explain why changes were made
3. **Keep changes small**: Easier to review thoroughly
4. **Respond constructively**: Don't take feedback personally
5. **Document decisions**: Explain architectural choices

## Automated Checks

```javascript
// ESLint configuration example
// .eslintrc.json
{
  "rules": {
    "no-unused-vars": "error",
    "no-console": "warn",
    "eqeqeq": "error",
    "no-var": "error",
    "prefer-const": "error"
  }
}

// Prettier configuration
// .prettierrc
{
  "semi": true,
  "singleQuote": true,
  "tabWidth": 2,
  "trailingComma": "es5"
}
```

## Code Review Tools

| Tool | Purpose |
|------|---------|
| ESLint | Code quality analysis |
| Prettier | Code formatting |
| SonarQube | Security vulnerabilities |
| CodeClimate | Maintainability metrics |

## Common Mistakes in Reviews

```javascript
// ❌ Review: Nitpicking style when logic is wrong
// Focus on functionality first, style second

// ❌ Review: Not testing the code
// Always verify code works as expected

// ❌ Review: Approving too quickly
// Take time to understand the changes

// ❌ Review: Being too harsh
// Remember there's a person behind the code
```

## Related Topics

- [[Coding-Standards]] - Team coding conventions
- [[Testing]] - Automated testing practices
- [[Debugging]] - Finding and fixing issues
- [[Refactoring]] - Improving existing code
- [[Git-Workflow]] - Version control best practices
- [[Documentation]] - Writing meaningful docs

## Quick Revision

**Review Focus Areas:**
1. Functionality - Does it work correctly?
2. Readability - Is it easy to understand?
3. Maintainability - Will it be easy to modify?
4. Security - Are there vulnerabilities?
5. Performance - Are there bottlenecks?

**Best Practices:**
- Review in small batches
- Use automated tools first
- Be constructive and specific
- Test the code when possible
- Document decisions and rationale

**Remember**: Good code review is about improving code quality and team knowledge, not finding fault.