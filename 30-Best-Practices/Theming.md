# Theming

## Definition

Theming creates **customizable visual styles**.

## CSS Variables

```css
:root {
    --primary: #007bff;
    --secondary: #6c757d;
}

.button {
    background: var(--primary);
}

/* Dark theme */
[data-theme="dark"] {
    --primary: #0056b3;
}
```

## Quick Revision

- CSS variables for theme values
- Toggle themes by changing variables
- Store preference in localStorage
- Use CSS custom properties

---

## Related Topics

- [[What-is-CSS]] - [[What-is-CSS|CSS]]
- [[Theming]] - [[Theming|Theming]]
- [[What-is-LocalStorage]] - [[What-is-LocalStorage|LocalStorage]]
- [[CSS-Basics]] - [[CSS-Basics|CSS basics]]
