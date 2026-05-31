# What-is-Animation

## Definition

Animation creates **visual changes** over time.

## CSS Animation

```css
.box {
    animation: slide 1s ease-in-out;
}

@keyframes slide {
    from { transform: translateX(0); }
    to { transform: translateX(100px); }
}
```

## JavaScript Animation

```javascript
element.animate([
    { transform: 'translateX(0)' },
    { transform: 'translateX(100px)' }
], {
    duration: 1000,
    easing: 'ease-in-out'
});
```

## Quick Revision

- CSS: @keyframes
- JavaScript: Web Animations API
- requestAnimationFrame for smooth
- Use for: transitions, effects

---

## Related Topics

- [[Animation]] - [[Animation|Animation]]
- [[What-is-Animation]] - [[What-is-Animation|Animation]]
- [[RequestAnimationFrame]] - [[RequestAnimationFrame|requestAnimationFrame]]
- [[Optimize-Rendering]] - [[Optimize-Rendering|Optimizing rendering]]
