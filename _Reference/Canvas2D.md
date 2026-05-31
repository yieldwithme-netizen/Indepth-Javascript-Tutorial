# Canvas 2D API

## Definition
The Canvas 2D API provides a JavaScript interface for drawing 2D graphics on an HTML `<canvas>` element. It enables rendering shapes, text, images, and other graphical objects directly in the browser.

## Syntax
```html
<canvas id="myCanvas" width="400" height="300"></canvas>
```

```javascript
const canvas = document.getElementById('myCanvas');
const ctx = canvas.getContext('2d');
```

## Drawing Shapes

### Rectangle
```javascript
// Filled rectangle
ctx.fillStyle = 'blue';
ctx.fillRect(10, 10, 150, 100);

// Outlined rectangle
ctx.strokeStyle = 'red';
ctx.lineWidth = 2;
ctx.strokeRect(10, 130, 150, 100);

// Clear a rectangular area
ctx.clearRect(10, 10, 50, 50);
```

### Lines and Paths
```javascript
ctx.beginPath();
ctx.moveTo(50, 50);
ctx.lineTo(200, 50);
ctx.lineTo(200, 150);
ctx.strokeStyle = 'green';
ctx.lineWidth = 3;
ctx.stroke();

// Close the path
ctx.beginPath();
ctx.moveTo(50, 200);
ctx.lineTo(200, 200);
ctx.lineTo(200, 280);
ctx.closePath();
ctx.fillStyle = 'rgba(0, 128, 255, 0.5)';
ctx.fill();
ctx.stroke();
```

### Circles and Arcs
```javascript
// Full circle
ctx.beginPath();
ctx.arc(200, 150, 80, 0, 2 * Math.PI);
ctx.fillStyle = 'orange';
ctx.fill();
ctx.stroke();

// Arc (half circle)
ctx.beginPath();
ctx.arc(200, 150, 60, 0, Math.PI);
ctx.strokeStyle = 'purple';
ctx.lineWidth = 4;
ctx.stroke();
```

## Drawing Text
```javascript
// Filled text
ctx.font = '30px Arial';
ctx.fillStyle = 'black';
ctx.textAlign = 'center';
ctx.fillText('Hello Canvas', 200, 50);

// Stroked text
ctx.strokeStyle = 'blue';
ctx.lineWidth = 1;
ctx.strokeText('Outline Text', 200, 100);
```

## Images
```javascript
const img = new Image();
img.src = 'photo.png';
img.onload = function() {
  ctx.drawImage(img, 0, 0);
  // Draw with size
  ctx.drawImage(img, 0, 0, 200, 200);
  // Draw from source to destination
  ctx.drawImage(img, 50, 50, 100, 100, 0, 0, 300, 300);
};
```

## Gradients
```javascript
// Linear gradient
const linearGrad = ctx.createLinearGradient(0, 0, 200, 0);
linearGrad.addColorStop(0, 'red');
linearGrad.addColorStop(0.5, 'yellow');
linearGrad.addColorStop(1, 'green');
ctx.fillStyle = linearGrad;
ctx.fillRect(10, 10, 300, 50);

// Radial gradient
const radialGrad = ctx.createRadialGradient(150, 150, 10, 150, 150, 100);
radialGrad.addColorStop(0, 'white');
radialGrad.addColorStop(1, 'blue');
ctx.fillStyle = radialGrad;
ctx.beginPath();
ctx.arc(150, 150, 100, 0, 2 * Math.PI);
ctx.fill();
```

## Transforms
```javascript
// Translate
ctx.translate(100, 100);

// Rotate (in radians)
ctx.rotate(Math.PI / 4);

// Scale
ctx.scale(1.5, 1.5);

// Save and restore state
ctx.save();
ctx.fillStyle = 'red';
ctx.fillRect(0, 0, 50, 50);
ctx.restore();
ctx.fillRect(60, 0, 50, 50);
```

## Animation Example
```javascript
let x = 0;
function animate() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);
  ctx.fillStyle = 'red';
  ctx.beginPath();
  ctx.arc(x, 150, 20, 0, 2 * Math.PI);
  ctx.fill();
  x = (x + 2) % canvas.width;
  requestAnimationFrame(animate);
}
animate();
```

## Common Use Cases
- Data visualization and charts
- Game development
- Interactive graphics and animations
- Image manipulation
- Drawing tools and whiteboards
- Dynamic backgrounds

## Common Mistakes
- Forgetting to call `getContext('2d')` before drawing
- Not handling image `onload` before calling `drawImage`
- Mixing canvas CSS size with attribute dimensions
- Not clearing the canvas before redrawing in animations
- Forgetting that transforms are cumulative

## Related Topics
- [[HTML5]]
- [[DOM-Manipulation]]
- [[SVG]]
- [[Animation]]
- [[WebGL]]
- [[Event-Handling]]

## Quick Revision
- Canvas uses a drawing context obtained via `getContext('2d')`
- All drawing operations use the context object
- Paths must begin with `beginPath()` and end with `stroke()` or `fill()`
- Use `requestAnimationFrame` for smooth animations
- `save()` and `restore()` manage drawing state
