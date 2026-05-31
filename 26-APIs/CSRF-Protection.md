# CSRF Protection

## Definition

CSRF protection **prevents cross-site request forgery** attacks.

## Express Setup

```javascript
const csrf = require('csurf');
const csrfProtection = csrf({ cookie: true });

app.use(csrfProtection);

app.get('/form', (req, res) => {
    res.render('form', { csrfToken: req.csrfToken() });
});
```

## Quick Revision

- CSRF = cross-site request forgery
- Use CSRF tokens
- Validate tokens on server
- Use SameSite cookies

---

## Related Topics

- [[What-is-CSRF]] - [[What-is-CSRF|CSRF]]
- [[CSRF-Protection]] - [[CSRF-Protection|CSRF protection]]
- [[What-is-XSS]] - [[What-is-XSS|XSS]]
- [[Security-Best-Practices]] - [[Security-Best-Practices|Security]]
