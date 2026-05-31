# style Property

## Definition

The `style` property gives you access to inline styles on an element. You can get or set individual CSS properties directly using JavaScript. For reading computed styles, use `getComputedStyle()` instead.

## Basic Syntax

```javascript
// Set inline style
element.style.color = 'red';
element.style.fontSize = '20px';

// Get inline style
console.log(element.style.color); // "red"
```

## Setting Styles

```javascript
const box = document.getElementById('box');

// Set individual properties (camelCase)
box.style.backgroundColor = 'blue';
box.style.color = 'white';
box.style.padding = '20px';
box.style.borderRadius = '8px';
box.style.boxShadow = '0 2px 4px rgba(0,0,0,0.1)';

// Set multiple properties
Object.assign(box.style, {
  backgroundColor: 'blue',
  color: 'white',
  padding: '20px'
});
```

## CSS Property Names in JavaScript

```javascript
// CSS uses kebab-case, JavaScript uses camelCase
// CSS                    → JavaScript
element.style.background-color → element.style.backgroundColor
element.style.font-size       → element.style.fontSize
element.style.margin-top      → element.style.marginTop
element.style.border-radius   → element.style.borderRadius
```

## Getting Computed Styles

```javascript
const element = document.getElementById('card');

// inlineStyle only reads inline styles
console.log(element.style.color); // "" (empty if not inline)

// getComputedStyle reads all applied styles
const computed = getComputedStyle(element);
console.log(computed.color);        // "rgb(0, 0, 0)"
console.log(computed.fontSize);     // "16px"
console.log(computed.display);      // "block"
```

## Practical Examples

### Animated Progress Bar

```javascript
function setProgress(element, percent) {
  element.style.width = `${percent}%`;
  element.style.transition = 'width 0.3s ease';

  if (percent < 30) {
    element.style.backgroundColor = '#ef4444';
  } else if (percent < 70) {
    element.style.backgroundColor = '#f59e0b';
  } else {
    element.style.backgroundColor = '#22c55e';
  }
}
```

### Dynamic Tooltip Positioning

```javascript
function positionTooltip(tooltip, target) {
  const rect = target.getBoundingClientRect();

  tooltip.style.position = 'absolute';
  tooltip.style.top = `${rect.top - tooltip.offsetHeight - 10}px`;
  tooltip.style.left = `${rect.left + (rect.width / 2) - (tooltip.offsetWidth / 2)}px`;
}
```

### Responsive Font Size

```javascript
function adjustFontSize(element) {
  const containerWidth = element.parentElement.offsetWidth;
  const fontSize = Math.max(12, Math.min(24, containerWidth / 20));

  element.style.fontSize = `${fontSize}px`;
}

window.addEventListener('resize', () => {
  adjustFontSize(document.getElementById('title'));
});
```

### Dynamic Background Based on Input

```javascript
const colorInput = document.getElementById('color-picker');
const preview = document.getElementById('preview');

colorInput.addEventListener('input', (e) => {
  preview.style.backgroundColor = e.target.value;
  preview.style.transition = 'background-color 0.2s ease';
});
```

### Show/Hide Elements

```javascript
function show(element) {
  element.style.display = 'block';
  element.style.opacity = '1';
}

function hide(element) {
  element.style.display = 'none';
}

function fadeIn(element) {
  element.style.opacity = '0';
  element.style.display = 'block';
  element.style.transition = 'opacity 0.3s ease';

  requestAnimationFrame(() => {
    element.style.opacity = '1';
  });
}

function fadeOut(element) {
  element.style.transition = 'opacity 0.3s ease';
  element.style.opacity = '0';

  setTimeout(() => {
    element.style.display = 'none';
  }, 300);
}
```

## style vs classList vs className

```javascript
// style — inline styles (highest specificity)
element.style.color = 'red';

// classList — toggle CSS classes (recommended)
element.classList.add('active');

// className — set all classes (replaces)
element.className = 'box active';
```

### When to Use What

```javascript
// Use style for:
// 1. Dynamic values (user input, calculations)
// 2. One-off style changes
element.style.width = `${value}px`;

// Use classList for:
// 1. Predefined styles
// 2. State management
element.classList.toggle('hidden');

// Use className for:
// 1. Resetting all classes
element.className = 'card active';
```

## Setting CSS Variables

```javascript
const element = document.getElementById('theme');

// Set CSS custom properties
element.style.setProperty('--primary-color', '#3b82f6');
element.style.setProperty('--spacing', '16px');

// Get CSS custom properties
const primary = getComputedStyle(element).getPropertyValue('--primary-color');
```

## Important Properties

| Property | JavaScript Name | Example |
|----------|-----------------|---------|
| `background-color` | `backgroundColor` | `'blue'` |
| `font-size` | `fontSize` | `'16px'` |
| `margin-top` | `marginTop` | `'10px'` |
| `border-radius` | `borderRadius` | `'8px'` |
| `display` | `display` | `'flex'` |
| `transition` | `transition` | `'all 0.3s'` |
| `transform` | `transform` | `'rotate(45deg)'` |
| `opacity` | `opacity` | `'0.5'` |

## Common Use Cases

| Use Case | Code |
|----------|------|
| Set color | `el.style.color = 'red'` |
| Set size | `el.style.width = '100px'` |
| Set display | `el.style.display = 'none'` |
| Get computed | `getComputedStyle(el).color` |
| Set CSS variable | `el.style.setProperty('--x', 'val')` |

## Common Mistakes to Avoid

1. **Using style for everything** — Prefer classes for reusable styles
2. **Reading computed styles via style** — Use `getComputedStyle()`
3. **Forgetting units** — Most properties need `px`, `%`, etc.

```javascript
// WRONG: Missing units
element.style.width = 100;

// RIGHT: Include units
element.style.width = '100px';

// WRONG: Reading computed style via style property
console.log(element.style.color); // May be empty

// RIGHT: Use getComputedStyle
console.log(getComputedStyle(element).color);
```

## Related Topics

- [[What-is-ClassList]]
- [[What-is-InnerHTML]]
- [[What-is-CreateElement]]
- [[What-is-Style]]

## Quick Revision

| Task | Method |
|------|--------|
| Set inline style | `el.style.prop = value` |
| Read inline style | `el.style.prop` |
| Read computed style | `getComputedStyle(el).prop` |
| Set CSS variable | `el.style.setProperty('--var', val)` |
| Remove style | `el.style.removeProperty('prop')` |
