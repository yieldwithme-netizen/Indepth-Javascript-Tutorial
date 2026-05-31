# DOM Ready

## Definition

DOM ready means the **HTML is fully parsed** and DOM is available.

## DOMContentLoaded

```javascript
document.addEventListener('DOMContentLoaded', () => {
    // Safe to manipulate DOM
    console.log('DOM ready');
});
```

## Quick Revision

- `DOMContentLoaded` fires when DOM ready
- Before images/styles load
- Use for DOM manipulation
- Faster than `load` event

---

## Related Topics

- [[What-is-DOM-Ready]] - [[What-is-DOM-Ready|DOM ready]]
- [[What-is-DOM]] - [[What-is-DOM|DOM]]
- [[What-is-Event]] - [[What-is-Event|Events]]
- [[Page-Load-Events]] - [[Page-Load-Events|Page load events]]
