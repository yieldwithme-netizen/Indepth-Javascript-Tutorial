# Dynamic Import

## Definition

Dynamic import loads modules **on demand** at runtime.

## Basic Syntax

```javascript
const module = await import('./module.js');
```

## In Functions

```javascript
button.addEventListener('click', async () => {
    const module = await import('./heavyModule.js');
    module.doSomething();
});
```

## Conditional Import

```javascript
if (condition) {
    const module = await import('./moduleA.js');
} else {
    const module = await import('./moduleB.js');
}
```

## Quick Revision

- `import()` returns Promise
- Load modules on demand
- Use for: code splitting, lazy loading
- Dynamic: can use variables in path

---

## Related Topics

- [[What-is-DynamicImport]] - [[What-is-DynamicImport|Dynamic import]]
- [[Dynamic-Import]] - [[Dynamic-Import|Dynamic import]]
- [[What-is-Module]] - [[What-is-Module|Modules]]
- [[Lazy-Load]] - [[Lazy-Load|Lazy loading]]
