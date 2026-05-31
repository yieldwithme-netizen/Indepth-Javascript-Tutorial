# Role-Based Access Control

## Definition

RBAC **restricts access** based on user roles.

## Example

```javascript
function checkRole(role) {
    return (req, res, next) => {
        if (req.user.role !== role) {
            return res.status(403).send('Forbidden');
        }
        next();
    };
}

app.get('/admin', checkRole('admin'), (req, res) => {
    res.send('Admin dashboard');
});
```

## Quick Revision

- RBAC = access by role
- Define roles and permissions
- Check role in middleware
- Return 403 for unauthorized

---

## Related Topics

- [[What-is-Authorization]] - [[What-is-Authorization|Authorization]]
- [[Role-Based-Access-Control]] - [[Role-Based-Access-Control|RBAC]]
- [[What-is-Authentication]] - [[What-is-Authentication|Authentication]]
- [[What-is-API]] - [[What-is-API|APIs]]
