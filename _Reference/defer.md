# The defer Attribute

## Definition

The `defer` attribute is a boolean attribute used on `<script>` tags in HTML. When specified, it tells the browser to download the script in parallel while parsing the HTML document, but execute it only after the HTML has been fully parsed. This prevents the script from blocking page rendering.

## Syntax

```html
<script src="script.js" defer></script>
```

## How defer Works

```html
<!DOCTYPE html>
<html>
<head>
  <!-- These scripts download in parallel with HTML parsing -->
  <!-- They execute in order, after the DOM is ready -->
  <script src="app.js" defer></script>
  <script src="utils.js" defer></script>
  <script src="init.js" defer></script>
</head>
<body>
  <h1>Page Content</h1>
  <div id="app"></div>
</body>
</html>
```

## Execution Order

```javascript
// app.js (loads first, executes first after DOM ready)
console.log('App loaded');

// utils.js (loads second, executes second)
console.log('Utils loaded');

// init.js (loads third, executes third)
console.log('Init loaded');
// Output: App loaded → Utils loaded → Init loaded
```

## Common Use Cases

### 1. External Libraries

```html
<!-- jQuery -->
<script src="https://code.jquery.com/jquery-3.6.0.min.js" defer></script>

<!-- Your code runs after jQuery loads -->
<script src="app.js" defer></script>
```

### 2. Page Initialization Scripts

```javascript
// main.js with defer - DOM is ready when this executes
document.addEventListener('DOMContentLoaded', () => {
  initNavigation();
  loadDynamicContent();
  setupEventListeners();
});
```

### 3. Analytics Scripts

```html
<!-- Analytics doesn't block page rendering -->
<script src="analytics.js" defer></script>
```

## defer vs async vs Regular Scripts

```html
<!-- Regular: Blocks HTML parsing until script downloads and executes -->
<script src="blocking.js"></script>

<!-- defer: Downloads in parallel, executes after DOM parsing -->
<script src="deferred.js" defer></script>

<!-- async: Downloads in parallel, executes as soon as downloaded -->
<script src="async-script.js" async></script>
```

## Common Mistakes

### 1. Using defer with inline scripts

```html
<!-- WRONG: defer doesn't work with inline scripts -->
<script defer>
  console.log('This will run immediately');
</script>

<!-- RIGHT: Use external file -->
<script src="script.js" defer></script>
```

### 2. Relying on script order without defer

```html
<!-- WRONG: Order not guaranteed without defer -->
<script src="first.js"></script>
<script src="second.js"></script>

<!-- RIGHT: defer guarantees order -->
<script src="first.js" defer></script>
<script src="second.js" defer></script>
```

### 3. Using defer for dynamic content scripts

```javascript
// WRONG: defer only works for initial page load
const script = document.createElement('script');
script.src = 'new.js';
script.defer = true; // Won't work as expected
document.body.appendChild(script);
```

## Browser Support

| Browser | Support |
|---------|---------|
| Chrome  | ✅ Yes  |
| Firefox | ✅ Yes  |
| Safari  | ✅ Yes  |
| Edge    | ✅ Yes  |

## Quick Revision Summary

- `defer` downloads scripts in parallel with HTML parsing
- Scripts execute **after** the DOM is fully parsed
- Scripts execute **in order** they appear in HTML
- Does not work with inline scripts
- Best for scripts that need the full DOM
- Improves page load performance

## Related Topics

- [[Async-Scripts]] - Learn about async attribute for scripts
- [[DOM-Manipulation]] - Working with the Document Object Model
- [[Event-Handling]] - Handling user interactions
- [[Module-Scripts]] - ES6 modules in the browser
- [[Page-Load-Events]] - Understanding browser load events
- [[Performance-Optimization]] - Web performance best practices
