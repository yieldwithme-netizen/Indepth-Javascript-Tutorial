# Custom Events

## Definition

Custom events **create and dispatch** your own events.

## Example

```javascript
// Create
const event = new CustomEvent('userLoggedIn', {
    detail: { name: "John" }
});

// Listen
element.addEventListener('userLoggedIn', (e) => {
    console.log(e.detail.name);
});

// Dispatch
element.dispatchEvent(event);
```

## Quick Revision

- CustomEvent for custom events
- `detail` for data
- `dispatchEvent()` to trigger
- Use for: component communication

---

## Related Topics

- [[What-is-Event]] - [[What-is-Event|Events]]
- [[Custom-Events]] - [[Custom-Events|Custom events]]
- [[Add-Listener]] - [[Add-Listener|Adding listeners]]
- [[Event-Bus]] - [[Event-Bus|Event bus]]
