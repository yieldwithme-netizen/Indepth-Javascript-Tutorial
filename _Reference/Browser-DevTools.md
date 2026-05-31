# Browser DevTools

## Definition
Browser DevTools are built-in web development tools in modern browsers that help developers debug, test, and optimize web applications. They provide panels for inspecting elements, debugging JavaScript, monitoring network requests, and more.

## Key Panels

### Elements Panel
```javascript
// Inspect elements
document.querySelector('#myElement').style.color = 'red';

// Modify styles in console
$0.style.backgroundColor = 'blue'; // $0 = currently selected element
```

### Console Panel
```javascript
// Basic logging
console.log('Hello');
console.warn('Warning');
console.error('Error');

// Table display
console.table([{ name: 'Alice', age: 30 }, { name: 'Bob', age: 25 }]);

// Group logs
console.group('User Info');
console.log('Name: Alice');
console.log('Age: 30');
console.groupEnd();

// Timing
console.time('loop');
for (let i = 0; i < 1000000; i++) {}
console.timeEnd('loop'); // loop: 2.5ms

// Trace
function a() { b(); }
function b() { c(); }
function c() { console.trace(); }
a(); // Shows call stack
```

### Sources Panel (Debugger)
```javascript
// Set breakpoints
debugger; // Pauses execution here

// Conditional breakpoints
// Right-click line → Add conditional breakpoint
// Expression: i === 50

// Watch expressions
// Add expressions in Watch panel
// Example: myArray.length
```

### Network Panel
```javascript
// Monitor requests
// 1. Open Network tab
// 2. Reload page
// 3. Click any request to see:
//    - Headers
//    - Preview/Response
//    - Timing
//    - Initiator (what triggered the request)

// Filter requests
// Type: XHR, JS, CSS, Img
// Status: 200, 404, 500
```

### Performance Panel
```javascript
// Record performance
// 1. Click Record
// 2. Perform actions
// 3. Stop recording
// 4. Analyze:
//    - FPS
//    - CPU usage
//    - Memory allocation

// Profile specific code
console.profile('My Function');
// ... code to profile
console.profileEnd('My Function');
```

### Memory Panel
```javascript
// Take heap snapshot
// 1. Open Memory tab
// 2. Select "Heap snapshot"
// 3. Click Take snapshot

// Compare snapshots
// 1. Take first snapshot
// 2. Perform action
// 3. Take second snapshot
// 4. Select "Comparison" view

// Detect memory leaks
// Look for detached DOM elements
// Check increasing object counts
```

### Application Panel
```javascript
// Inspect storage
localStorage.setItem('key', 'value');
sessionStorage.setItem('key', 'value');

// View cookies
document.cookie = 'name=Alice; path=/';

// Service workers
// Inspect registered service workers
// Update/unregister workers

// Cache storage
// View cached resources
```

## Useful Shortcuts

### Chrome DevTools
| Action | Shortcut |
|--------|----------|
| Open DevTools | F12 or Ctrl+Shift+I |
| Console | Ctrl+Shift+J |
| Elements | Ctrl+Shift+C |
| Search | Ctrl+Shift+F |
| Reload page | Ctrl+Shift+R |

### Firefox DevTools
| Action | Shortcut |
|--------|----------|
| Open DevTools | F12 or Ctrl+Shift+I |
| Console | Ctrl+Shift+K |
| Inspector | Ctrl+Shift+C |
| Debugger | Ctrl+Shift+Z |

## Console Methods
```javascript
// Clear console
console.clear();

// Count occurrences
console.count('label');
console.countReset('label');

// Assert
console.assert(1 === 2, 'Math is broken');

// Dir (detailed object view)
console.dir(document.body);

// Dirxml (DOM representation)
console.dirxml(document.body);

// Raw values
console.dir({ a: 1, b: [2, 3] });
console.dirraw({ a: 1, b: [2, 3] }); // Firefox only
```

## Debugging Tips
```javascript
// Copy to clipboard
copy(myObject);

// Query selector shortcuts
$('selector') // document.querySelector
$$('selector') // document.querySelectorAll
$_() // last evaluated expression

// Monitor function calls
monitor(fetch);
// fetch() called with: arguments

// Undocumented but useful
// Get event listeners
getEventListeners(document);
```

## Common Use Cases
- Debugging JavaScript errors
- Inspecting CSS styles
- Monitoring network requests
- Profiling performance
- Testing responsive designs
- Analyzing memory usage

## Common Mistakes
- **Not using breakpoints**: Relying only on `console.log`
- **Ignoring warnings**: Warnings often indicate issues
- **Not profiling**: Missing performance bottlenecks
- **Forgetting mobile debugging**: Use remote debugging for mobile

## Related Topics
- [[Console-Methods]]
- [[Debugging]]
- [[Performance-Optimization]]
- [[Network-Requests]]
- [[Memory-Leaks]]

## Quick Revision
- F12 or Ctrl+Shift+I opens DevTools
- Elements panel for DOM inspection
- Console for logging and evaluation
- Sources for debugging with breakpoints
- Network for request monitoring
- Performance for profiling
- Memory for detecting leaks
