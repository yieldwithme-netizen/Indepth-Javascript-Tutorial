# Touch Events

## Definition

Touch events fire when user **touches the screen**.

## Events

| Event | When |
|-------|------|
| touchstart | Touch begins |
| touchmove | Touch moves |
| touchend | Touch ends |

## Example

```javascript
element.addEventListener("touchstart", (e) => {
    const touch = e.touches[0];
    console.log(`Touch at: ${touch.clientX}`);
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
- [[What-is-TouchEvents]] - [[What-is-TouchEvents|Touch events]]
- [[Touch-Events]] - [[Touch-Events|Touch events]]
- [[What-is-MouseEvent]] - [[What-is-MouseEvent|Mouse events]]
