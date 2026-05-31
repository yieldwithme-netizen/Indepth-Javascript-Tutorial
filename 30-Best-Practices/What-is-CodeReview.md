# What is Code Review?

## Definition

Code review is the **systematic examination of source code** by peers to find bugs, improve code quality, and share knowledge.

## Why Code Review Matters

| Benefit | Description |
|---------|-------------|
| **Catch Bugs** | Find errors before production |
| **Share Knowledge** | Team learns from each other |
| **Maintain Standards** | Consistent code style |
| **Improve Design** | Better architecture decisions |
| **Document Code** | Create living documentation |

## Code Review Checklist

```markdown
## Functionality
- [ ] Does code work as expected?
- [ ] Are edge cases handled?
- [ ] Is error handling appropriate?

## Code Quality
- [ ] Is code readable?
- [ ] Are variable names descriptive?
- [ ] Is there unnecessary duplication?
- [ ] Are functions appropriately sized?

## Security
- [ ] Are inputs validated?
- [ ] Are secrets handled safely?
- [ ] Are there SQL injection risks?

## Testing
- [ ] Are there adequate tests?
- [ ] Do tests cover edge cases?
- [ ] Are tests maintainable?
```

## Reviewing Code Examples

```javascript
// BAD: Poor naming, no error handling
function calc(a, b) {
  return a / b;
}

// GOOD: Descriptive names, error handling
function divide(dividend, divisor) {
  if (divisor === 0) {
    throw new Error("Division by zero");
  }
  return dividend / divisor;
}
```

## Providing Constructive Feedback

```javascript
// BAD: Vague feedback
// "This is wrong"

// GOOD: Specific, actionable feedback
// "This function could throw if user is null.
//  Consider adding null check: if (!user) return null;"

// BAD: Personal criticism
// "You always write bad code"

// GOOD: Focus on the code
// "Consider extracting this logic into a separate function
//  for better readability and reusability"
```

## GitHub Pull Request Review

```markdown
## PR Description
- Clear summary of changes
- Link to related issue
- Screenshots if UI changed

## Review Comments
- Line-by-line feedback
- Suggest code changes
- Ask questions

## Approval Process
- Request changes
- Approve
- Comment only
```

## Common Issues to Look For

```javascript
// 1. Security issues
const query = `SELECT * FROM users WHERE id = ${userId}`; // SQL injection!

// Better:
const query = "SELECT * FROM users WHERE id = ?";
db.query(query, [userId]);

// 2. Performance issues
users.forEach((user) => {
  await sendEmail(user.email); // Sequential!
});

// Better:
await Promise.all(users.map((user) => sendEmail(user.email)));

// 3. Error handling
async function fetchData() {
  const response = await fetch(url);
  return response.json(); // No error handling
}

// Better:
async function fetchData() {
  try {
    const response = await fetch(url);
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    return await response.json();
  } catch (error) {
    console.error("Failed to fetch:", error);
    throw error;
  }
}
```

## Code Review Tools

| Tool | Platform |
|------|----------|
| GitHub Pull Requests | GitHub |
| GitLab Merge Requests | GitLab |
| Bitbucket Pull Requests | Bitbucket |
| Gerrit | Self-hosted |
| Crucible | Atlassian |

## Common Mistakes

```javascript
// BAD: Not reviewing at all
// Just approving without looking

// BAD: Being too pedantic
// "Your variable name is one letter different from mine"

// BAD: Not explaining reasoning
// "Change this"

// GOOD: Explain why
// "This could cause a race condition. Consider using a mutex."

// GOOD: Suggest alternatives
// "This works, but we could also use Array.reduce() here for clarity"
```

## Quick Revision

- Code review catches bugs and shares knowledge
- Use a checklist for consistency
- Provide specific, actionable feedback
- Focus on code, not people
- Review for security, performance, and readability
- Always explain the reasoning behind suggestions

---

## Related Topics

- [[What-is-CleanCode]] - Clean code principles
- [[What-is-DRY]] - DRY principle
- [[What-is-SOLID]] - SOLID principles
- [[Debug-JavaScript]] - Debugging
- [[Use-Git]] - Git workflows
- [[What-is-Documentation]] - Documentation