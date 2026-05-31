# SVG

## Definition

SVG (Scalable Vector Graphics) creates **vector images** with XML.

## Example

```html
<svg width="100" height="100">
    <circle cx="50" cy="50" r="40" stroke="green" stroke-width="4" fill="yellow" />
</svg>
```

## JavaScript with SVG

```javascript
const svg = document.querySelector('svg');
const circle = document.createElementNS('http://www.w3.org/2000/svg', 'circle');
circle.setAttribute('cx', '50');
circle.setAttribute('cy', '50');
circle.setAttribute('r', '40');
svg.appendChild(circle);
```

## Quick Revision

- SVG = vector graphics
- XML-based syntax
- Scalable without quality loss
- Use for: icons, charts, illustrations

---

## Related Topics

- [[What-is-SVG]] - [[What-is-SVG|SVG]]
- [[SVG]] - [[SVG|SVG]]
- [[What-is-Canvas]] - [[What-is-Canvas|Canvas]]
- [[What-is-DOM]] - [[What-is-DOM|DOM]]
