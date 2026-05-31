# Responsive Design

## Definition

Responsive design **adapts to different screen sizes**.

## CSS

```css
/* Mobile first */
.container {
    width: 100%;
    padding: 10px;
}

/* Tablet */
@media (min-width: 768px) {
    .container { width: 750px; }
}

/* Desktop */
@media (min-width: 1024px) {
    .container { width: 1000px; }
}
```

## Quick Revision

- Mobile-first approach
- Media queries for breakpoints
- Flexible layouts
- Responsive images

---

## Related Topics

- [[What-is-CSS]] - [[What-is-CSS|CSS]]
- [[Responsive-Design]] - [[Responsive-Design|Responsive design]]
- [[What-is-DOM]] - [[What-is-DOM|DOM]]
- [[CSS-Basics]] - [[CSS-Basics|CSS basics]]
