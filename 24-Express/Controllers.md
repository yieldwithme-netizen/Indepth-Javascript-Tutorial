# Controllers

## Definition

Controllers **handle HTTP requests** and return responses.

## Express Example

```javascript
// controllers/userController.js
exports.getUsers = async (req, res) => {
    const users = await User.find();
    res.json(users);
};

exports.createUser = async (req, res) => {
    const user = await User.create(req.body);
    res.status(201).json(user);
};
```

## Quick Revision

- Controllers handle requests
- Process data
- Return responses
- Use in routes

---

## Related Topics

- [[What-is-Express]] - [[What-is-Express|Express]]
- [[What-is-Routes]] - [[What-is-Routes|Routes]]
- [[Controllers]] - [[Controllers|Controllers]]
- [[What-is-Middleware]] - [[What-is-Middleware|Middleware]]
