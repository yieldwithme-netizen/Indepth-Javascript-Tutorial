# Touch Events

## Definition

Touch events fire when the user **touches the screen**.

## Events

| Event | When |
|-------|------|
| touchstart | Touch begins |
| touchmove | Touch moves |
| touchend | Touch ends |
| touchcancel | Touch cancelled |

## Examples

```javascript
element.addEventListener("touchstart", (e) => {
    console.log("Touch started");
});

element.addEventListener("touchmove", (e) => {
    const touch = e.touches[0];
    console.log(`Touch at: ${touch.clientX}, ${touch.clientY}`);
});

element.addEventListener("touchend", (e) => {
    console.log("Touch ended");
});
```

## Quick Revision

- touchstart: touch begins
- touchmove: touch moves
- touchend: touch ends
- Use for: mobile interactions

---

## Related Topics

- [[What-is-TouchEvents]] - [[What-is-TouchEvents|Touch events]]
- [[Touch-Events]] - [[Touch-Events|Touch events]]
- [[What-is-Event]] - [[What-is-Event|Events]]
- [[What-is-MouseEvent]] - [[What-is-MouseEvent|Mouse events]]
