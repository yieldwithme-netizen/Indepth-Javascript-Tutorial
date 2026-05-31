# How to Optimize Rendering

## Definition

Rendering optimization minimizes layout shifts, reduces repaints/reflows, and ensures smooth 60fps visual performance in the browser.

## The Rendering Pipeline

1. **JavaScript** → Style calculations
2. **Style** → Layout (geometry)
3. **Layout** → Paint (pixels)
4. **Paint** → Composite (layers)

## Key Optimization Techniques

### Minimize Reflows

```javascript
// Bad: Triggers layout thrashing
for (let i = 0; i < 100; i++) {
  element.style.width = `${i}px`; // forces layout each time
}

// Good: Batch DOM reads and writes
const widths = [];
for (let i = 0; i < 100; i++) {
  widths.push(`${i}px`);
}
element.style.width = widths.join(" ");
```

### Use `requestAnimationFrame`

```javascript
function animate() {
  // Perform animation
  element.style.transform = `translateX(${x}px)`;
  requestAnimationFrame(animate);
}
requestAnimationFrame(animate);
```

### Use `requestIdleCallback` for Non-Urgent Work

```javascript
requestIdleCallback((deadline) => {
  while (deadline.timeRemaining() > 0) {
    processNonUrgentTask();
  }
});
```

### CSS Containment

```css
.expensive-component {
  contain: layout style paint;
}
```

### Will-Change for GPU Acceleration

```css
.animated-element {
  will-change: transform, opacity;
}
```

### Virtual Scrolling (Long Lists)

```javascript
function createVirtualList(items, container, itemHeight) {
  container.addEventListener("scroll", () => {
    const start = Math.floor(container.scrollTop / itemHeight);
    const end = start + Math.ceil(container.clientHeight / itemHeight);
    renderItems(items.slice(start, end));
  });
}
```

### Debounce/Throttle Events

```javascript
function debounce(fn, delay) {
  let timer;
  return (...args) => {
    clearTimeout(timer);
    timer = setTimeout(() => fn(...args), delay);
  };
}

window.addEventListener(
  "resize",
  debounce(() => updateLayout(), 100)
);
```

### Use `IntersectionObserver` for Lazy Rendering

```javascript
const observer = new IntersectionObserver((entries) => {
  entries.forEach((entry) => {
    if (entry.isIntersecting) {
      renderComponent(entry.target);
    }
  });
});

document.querySelectorAll(".lazy-section").forEach((el) => observer.observe(el));
```

## Common Use Cases

- **Infinite scroll**: Virtualize large lists
- **Animation**: Smooth 60fps transitions
- **Image galleries**: Lazy load off-screen images
- **Dashboard widgets**: Defer non-visible updates

## Common Mistakes

```javascript
// Mistake: Reading layout after write
element.style.width = "100px";
const height = element.offsetHeight; // Forces reflow

// Fix: Read first, then write
const height = element.offsetHeight;
element.style.width = "100px";
```

## Related Topics

- [[Measure-Performance]]
- [[What-is-LazyLoading]]
- [[What-is-CodeSplitting]]

## Quick Revision

| Technique | Benefit |
|-----------|---------|
| Batch DOM ops | Reduce layout thrashing |
| `requestAnimationFrame` | Sync with display refresh |
| CSS containment | Isolate reflows |
| Virtual scroll | Handle large lists |
| Debounce/Throttle | Limit event frequency |
