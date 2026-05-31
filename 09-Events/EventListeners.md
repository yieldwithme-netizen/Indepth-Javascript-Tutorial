# EventListeners

## Definition

Event listeners **attach functions** to DOM events.

## Basic Syntax

```javascript
element.addEventListener("click", handler);
element.removeEventListener("click", handler);
```

## Options

```javascript
element.addEventListener("click", handler, {
    once: true,      // Run once
    capture: true,   // Capture phase
    passive: true    // No preventDefault
});
```

## Quick Revision

- `addEventListener()` to attach
- `removeEventListener()` to detach
- Options: once, capture, passive
- Handler receives event object

---

## Related Topics

- [[What-is-Event]] - [[What-is-Event|Events]]
- [[Add-Listener]] - [[Add-Listener|Adding listeners]]
- [[EventListeners]] - [[EventListeners|Event listeners]]
- [[Handle-Clicks]] - [[Handle-Clicks|Click handling]]
