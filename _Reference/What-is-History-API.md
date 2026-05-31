# What-is-History-API

## Definition

History API manipulates **browser session history**.

## Example

```javascript
history.pushState({ page: 1 }, "Page 1", "/page1");
history.replaceState({ page: 2 }, "Page 2", "/page2");
history.back();
history.forward();
```

## Quick Revision

- pushState: add to history
- replaceState: replace current
- back/forward: navigate
- Use for: SPA routing

---

## Related Topics

- [[What-is-History-API]] - [[What-is-History-API|History API]]
- [[What-is-History-API]] - [[What-is-History-API|History API]]
- [[History API]] - [[History API|History API]]
- [[SPA Architecture]] - [[SPA Architecture|SPA architecture]]
