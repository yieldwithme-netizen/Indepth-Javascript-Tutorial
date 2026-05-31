# What-is-Canvas

## Definition

Canvas draws **graphics** on a web page.

## Example

```javascript
const canvas = document.getElementById('canvas');
const ctx = canvas.getContext('2d');

// Draw rectangle
ctx.fillStyle = 'red';
ctx.fillRect(10, 10, 100, 100);

// Draw circle
ctx.beginPath();
ctx.arc(50, 50, 40, 0, Math.PI * 2);
ctx.fill();
```

## Quick Revision

- Canvas for 2D/3D graphics
- getContext('2d') for 2D
- Draw shapes, images, text
- Use for: games, charts, animations

---

## Related Topics

- [[What-is-Canvas]] - [[What-is-Canvas|Canvas]]
- [[What-is-Canvas]] - [[What-is-Canvas|Canvas]]
- [[Canvas2D]] - [[Canvas2D|Canvas 2D]]
- [[What-is-DOM]] - [[What-is-DOM|DOM]]
