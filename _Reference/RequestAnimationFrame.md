# RequestAnimationFrame

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

// Start animation
requestAnimationFrame(animate);
```

## With Timestamp

```javascript
function animate(timestamp) {
    const elapsed = timestamp - startTime;
    const position = elapsed / 10; // smooth movement
    
    element.style.left = `${position}px`;
    
    if (position < 500) {
        requestAnimationFrame(animate);
    }
}

let startTime;
requestAnimationFrame((timestamp) => {
    startTime = timestamp;
    animate(timestamp);
});
```

## Cancel Animation

```javascript
let animationId;

function animate() {
    // ... animation code
    animationId = requestAnimationFrame(animate);
}

// Stop animation
cancelAnimationFrame(animationId);
```

## Quick Revision

- `requestAnimationFrame()` for smooth animations
- Syncs with browser refresh rate (60fps)
- Returns animation ID
- Use `cancelAnimationFrame()` to stop
- Better than `setTimeout()` for animations

---

## Related Topics

- [[What-is-RequestAnimationFrame]] - [[What-is-RequestAnimationFrame|requestAnimationFrame]]
- [[RequestAnimationFrame]] - [[RequestAnimationFrame|requestAnimationFrame]]
- [[Optimize-Rendering]] - [[Optimize-Rendering|Optimizing rendering]]
- [[What-is-DOM]] - [[What-is-DOM|DOM]]
