# requestAnimationFrame

## Definition

`requestAnimationFrame` **synchronizes animations** with the browser's refresh rate.

## Basic Usage

```javascript
function animate() {
    // Update animation
    element.style.left = `${position}px`;
    position += 2;
    
    // Continue animation
    requestAnimationFrame(animate);
}

// Start
requestAnimationFrame(animate);
```

## Cancel

```javascript
let id = requestAnimationFrame(animate);
cancelAnimationFrame(id);
```

## Quick Revision

- Syncs with 60fps refresh rate
- Returns animation ID
- Use `cancelAnimationFrame()` to stop
- Better than `setTimeout()` for animations

---

## Related Topics

- [[What-is-RequestAnimationFrame]] - [[What-is-RequestAnimationFrame|requestAnimationFrame]]
- [[RequestAnimationFrame]] - [[RequestAnimationFrame|requestAnimationFrame]]
- [[Optimize-Rendering]] - [[Optimize-Rendering|Optimizing rendering]]
