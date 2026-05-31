# History API

## Definition

History API manipulates the **browser session history**.

## Basic Usage

```javascript
// Add to history
history.pushState({ page: 1 }, "Page 1", "/page1");

// Replace current
history.replaceState({ page: 2 }, "Page 2", "/page2");

// Go back
history.back();

// Go forward
history.forward();

// Go to specific index
history.go(-2);
```

## Quick Revision

- `pushState()`: add to history
- `replaceState()`: replace current
- `back()`: go back
- `forward()`: go forward
- Use for: SPA routing

---

## Related Topics

- [[What-is-History-API]] - [[What-is-History-API|History API]]
- [[History API]] - [[History API|History API]]
- [[What-is-Routing]] - [[What-is-Routing|Routing]]
- [[SPA Architecture]] - [[SPA Architecture|SPA architecture]]
