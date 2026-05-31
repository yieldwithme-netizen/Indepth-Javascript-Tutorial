# What is Clean Code?

## Definition

Clean code is **easy to read, understand, and maintain**.

## Principles

### 1. Meaningful Names

```javascript
// ❌ Bad
const d = new Date();
const x = users.filter(u => u.a > 18);

// ✅ Good
const currentDate = new Date();
const adultUsers = users.filter(user => user.age > 18);
```

### 2. Small Functions

```javascript
// ❌ Bad
function processUser(user) {
    // 100 lines of code
}

// ✅ Good
function validateUser(user) { }
function saveUser(user) { }
function sendWelcomeEmail(user) { }
```

### 3. DRY (Don't Repeat Yourself)

```javascript
// ❌ Bad
const area1 = radius * radius * Math.PI;
const area2 = radius * radius * Math.PI;

// ✅ Good
function circleArea(radius) {
    return radius * radius * Math.PI;
}
```

### 4. Single Responsibility

```javascript
// ❌ Bad
function createUserAndSendEmail(user) { }

// ✅ Good
function createUser(user) { }
function sendEmail(user) { }
```

## Quick Revision

- Clean code = readable and maintainable
- Use meaningful names
- Keep functions small
- Follow DRY principle
- Single responsibility

---

## Related Topics

- [[What-is-CleanCode]] - Clean code overview
- [[What-is-DRY]] - DRY principle
- [[What-is-SOLID]] - SOLID principles
- [[What-is-CodeReview]] - Code review
- [[What-is-Debugging]] - Debugging
