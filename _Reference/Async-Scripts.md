# Async Scripts

## Definition

Async scripts load JavaScript files **asynchronously** without blocking HTML parsing.

## Script Loading Methods

```html
<!-- Blocking: pauses HTML parsing -->
<script src="app.js"></script>

<!-- Async: loads in parallel, executes immediately -->
<script src="app.js" async></script>

<!-- Defer: loads in parallel, executes after HTML parsed -->
<script src="app.js" defer></script>
```

## async vs defer

| Feature | async | defer |
|---------|-------|-------|
| Loads | Parallel | Parallel |
| Executes | Immediately when loaded | After HTML parsed |
| Order | Not guaranteed | Guaranteed |
| Use case | Independent scripts | DOM-dependent scripts |

## Examples

```html
<!-- Analytics (no dependencies) -->
<script src="analytics.js" async></script>

<!-- App code (depends on DOM) -->
<script src="app.js" defer></script>

<!-- Module scripts (always deferred) -->
<script type="module" src="app.js"></script>
```

## Quick Revision

- `async` = load and run immediately
- `defer` = load now, run after HTML parsed
- `type="module"` = deferred by default
- Use `async` for independent scripts
- Use `defer` for DOM-dependent scripts

---

## Related Topics

- [[Link-JS-HTML]] - [[Link-JS-HTML|Linking JS to HTML]]
- [[What-is-Module]] - [[What-is-Module|Modules]]
- [[defer]] - [[defer|Defer attribute]]
- [[What-is-DOM]] - [[What-is-DOM|DOM]]
