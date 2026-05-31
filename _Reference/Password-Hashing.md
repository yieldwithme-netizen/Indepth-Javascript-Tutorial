# Password Hashing

## Definition

Password hashing **converts passwords** to unreadable format for storage.

## Basic Example

```javascript
const bcrypt = require('bcrypt');

// Hash password
const hash = await bcrypt.hash("mypassword", 10);

// Verify password
const isValid = await bcrypt.compare("mypassword", hash);
```

## Quick Revision

- Hash: one-way conversion
- Use bcrypt for password hashing
- Never store plain text passwords
- Verify with compare()

---

## Related Topics

- [[What-is-Authentication]] - [[What-is-Authentication|Authentication]]
- [[Password-Hashing]] - [[Password-Hashing|Password hashing]]
- [[Store-Secrets]] - [[Store-Secrets|Storing secrets]]
- [[What-is-JWT]] - [[What-is-JWT|JWT]]
