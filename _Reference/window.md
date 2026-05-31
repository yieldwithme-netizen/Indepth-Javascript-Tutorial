# Window Object

The `window` object is the global object in browsers. It represents the browser window and provides access to the browser's APIs and the DOM.

## Global Scope

```javascript
// In browser, window is the global object
var globalVar = 'I am global';
console.log(window.globalVar); // 'I am global'

function globalFunc() {
  return 'I am a global function';
}
console.log(window.globalFunc()); // 'I am a global function'
```

## Window Properties

```javascript
// Window dimensions
console.log(window.innerWidth);   // Viewport width
console.log(window.innerHeight);  // Viewport height
console.log(window.outerWidth);   // Browser window width
console.log(window.outerHeight);  // Browser window height

// Screen info
console.log(window.screen.width);
console.log(window.screen.height);
console.log(window.screen.availWidth);

// URL info
console.log(window.location.href);
console.log(window.location.hostname);
console.log(window.location.pathname);
```

## Common Methods

```javascript
// Dialogs
alert('Hello!');
const result = confirm('Are you sure?');
const name = prompt('Enter your name:');

// Navigation
window.open('https://example.com');
window.close();

// Timing
setTimeout(() => console.log('After 1s'), 1000);
const id = setInterval(() => console.log('Every 1s'), 1000);
clearInterval(id);

// Scrolling
window.scrollTo(0, 100);
window.scrollBy(0, 50);
```

## Event Handling

```javascript
// Window events
window.addEventListener('load', () => {
  console.log('Page fully loaded');
});

window.addEventListener('resize', () => {
  console.log('Window resized');
});

window.addEventListener('scroll', () => {
  console.log('Scrolled');
});

window.addEventListener('beforeunload', (e) => {
  e.preventDefault();
  e.returnValue = '';
});
```

## Browser History

```javascript
// History API
window.history.pushState({ page: 1 }, 'Page 1', '/page1');
window.history.pushState({ page: 2 }, 'Page 2', '/page2');
window.history.back();
window.history.forward();
window.history.go(-2);
```

## Performance

```javascript
// Performance API
console.log(window.performance.now()); // High-res timestamp
const entries = window.performance.getEntriesByType('navigation');
console.log(entries[0].loadEventEnd - entries[0].fetchStart);
```

## Common Use Cases

- Managing browser windows
- Handling user interactions
- Navigating between pages
- Storing temporary data
- Communicating with browser APIs

## Common Mistakes

- Not checking if code runs in browser vs Node.js
- Overusing global variables on window
- Not handling window load event properly
- Memory leaks from uncleared timers
- Not using `window.` prefix when needed

## Related Topics

- [[DOM]]
- [[Events]]
- [[Browser APIs]]
- [[History API]]
- [[Fetch API]]

## Quick Revision

- `window` is the global object in browsers
- Provides access to browser APIs and DOM
- Properties: `innerWidth`, `innerHeight`, `location`
- Methods: `alert`, `confirm`, `setTimeout`, `setInterval`
- Events: `load`, `resize`, `scroll`, `beforeunload`
- History API for navigation control
