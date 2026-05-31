# Event Object

## Definition

Event object contains **information about the event**.

## Properties

```javascript
element.addEventListener('click', (e) => {
    console.log(e.type);     // "click"
    console.log(e.target);   // clicked element
    console.log(e.clientX);  // x position
    console.log(e.clientY);  // y position
});
```

## Quick Revision

- Event object passed to handler
- Properties: type, target, coordinates
- Use for event details

---

## Related Topics

- [[What-is-Event]] - [[What-is-Event|Events]]
- [[What-is-EventObject]] - [[What-is-EventObject|Event object]]
- [[Event Object]] - [[Event Object|Event object]]
- [[Use-Event-Props]] - [[Use-Event-Props|Event properties]]
