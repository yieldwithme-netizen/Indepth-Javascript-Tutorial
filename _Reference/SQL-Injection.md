# SQL Injection

## Definition
SQL injection is a security vulnerability that occurs when user input is improperly sanitized before being used in SQL queries. In JavaScript applications, this typically happens on the server-side (Node.js) when constructing database queries with user-provided data.

## How SQL Injection Works
An attacker can insert malicious SQL code into input fields, which gets executed against the database, potentially exposing, modifying, or deleting data.

## Vulnerable Code Example

```javascript
// ❌ VULNERABLE - Never do this!
const userInput = "'; DROP TABLE users; --";
const query = `SELECT * FROM users WHERE username = '${userInput}'`;
db.query(query);
```

## Secure Code Example

```javascript
// ✅ SAFE - Use parameterized queries
const userInput = req.body.username;
const query = "SELECT * FROM users WHERE username = ?";
db.query(query, [userInput]);

// ✅ SAFE - Using named parameters (PostgreSQL)
const query = "SELECT * FROM users WHERE username = $1";
db.query(query, [userInput]);

// ✅ SAFE - Using an ORM like Sequelize
const user = await User.findOne({
  where: { username: userInput }
});
```

## Common Use Cases
- User login/authentication systems
- Search functionality
- Data filtering and sorting
- CRUD operations

## Common Mistakes

| Mistake | Solution |
|---------|----------|
| Concatenating strings in queries | Use parameterized queries |
| Trusting user input | Validate and sanitize all input |
| Using ORM methods unsafely | Follow ORM documentation |
| Not using input validation | Implement server-side validation |

## Prevention Techniques
1. Use parameterized queries/prepared statements
2. Employ Object-Relational Mappers (ORMs)
3. Validate and sanitize all user inputs
4. Apply principle of least privilege to database accounts
5. Use stored procedures when possible

## Quick Revision Summary
- SQL injection exploits unsanitized user input in database queries
- Always use parameterized queries instead of string concatenation
- ORMs provide built-in protection when used correctly
- Validate all input on both client and server side
- Never concatenate user input directly into SQL strings

## Related Topics
- [[Node.js-Security]]
- [[Input-Validation]]
- [[Database-Connections]]
- [[Express-Middleware]]
- [[Authentication]]
