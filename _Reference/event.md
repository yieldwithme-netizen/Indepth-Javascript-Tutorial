# Events in JavaScript

## Definition

Events are actions or occurrences that happen in the browser or on a web page. JavaScript can respond to these events by executing code. Events can be triggered by user interactions (clicks, keystrokes), browser actions (page load, resize), or programmatic triggers.

## Event Flow

### 1. Capturing Phase

```javascript
// Events travel from root to target
document.addEventListener('click', () => {
  console.log('Document capturing');
}, true); // true = capturing phase

document.body.addEventListener('click', () => {
  console.log('Body capturing');
}, true);

document.querySelector('#app').addEventListener('click', () => {
  console.log('App capturing');
}, true);
```

### 2. Target Phase

```javascript
// Event reaches the target element
document.querySelector('#button').addEventListener('click', () => {
  console.log('Button clicked');
});
```

### 3. Bubbling Phase

```javascript
// Events travel from target back to root
document.querySelector('#button').addEventListener('click', () => {
  console.log('Button');
});

document.querySelector('#app').addEventListener('click', () => {
  console.log('App'); // Fires after button
});

document.body.addEventListener('click', () => {
  console.log('Body'); // Fires after app
});
```

## Event Object Properties

### 1. Basic Properties

```javascript
element.addEventListener('click', (event) => {
  // Event type
  console.log(event.type); // "click"

  // Target element (where event originated)
  console.log(event.target);

  // Current target (element with listener)
  console.log(event.currentTarget);

  // Timestamp
  console.log(event.timeStamp);

  // Is event being handled?
  console.log(event.eventPhase);
});
```

### 2. Mouse Properties

```javascript
element.addEventListener('click', (event) => {
  // Mouse position relative to viewport
  console.log('Client X:', event.clientX);
  console.log('Client Y:', event.clientY);

  // Mouse position relative to document
  console.log('Page X:', event.pageX);
  console.log('Page Y:', event.pageY);

  // Mouse position relative to target
  console.log('Offset X:', event.offsetX);
  console.log('Offset Y:', event.offsetY);

  // Screen position
  console.log('Screen X:', event.screenX);
  console.log('Screen Y:', event.screenY);
});
```

### 3. Keyboard Properties

```javascript
document.addEventListener('keydown', (event) => {
  // Key value
  console.log('Key:', event.key); // "a", "Enter", "Shift"

  // Key code (deprecated but still used)
  console.log('Code:', event.code); // "KeyA", "Enter", "ShiftLeft"

  // Modifier keys
  console.log('Alt:', event.altKey);
  console.log('Ctrl:', event.ctrlKey);
  console.log('Meta:', event.metaKey); // Cmd on Mac, Windows on PC
  console.log('Shift:', event.shiftKey);

  // Repeat key
  console.log('Repeat:', event.repeat);
});
```

### 4. Touch Properties

```javascript
element.addEventListener('touchstart', (event) => {
  // Touch points
  console.log('Touches:', event.touches.length);
  console.log('First touch:', event.touches[0].clientX);

  // Changed touches
  console.log('Changed:', event.changedTouches.length);
});
```

## Event Methods

### 1. preventDefault

```javascript
// Prevent form submission
document.querySelector('form').addEventListener('submit', (event) => {
  event.preventDefault();
  console.log('Form submission prevented');
});

// Prevent link navigation
document.querySelector('a').addEventListener('click', (event) => {
  event.preventDefault();
  console.log('Link click prevented');
});
```

### 2. stopPropagation

```javascript
// Stop event from bubbling up
const inner = document.querySelector('#inner');
const outer = document.querySelector('#outer');

inner.addEventListener('click', (event) => {
  event.stopPropagation();
  console.log('Inner clicked - propagation stopped');
});

outer.addEventListener('click', () => {
  console.log('Outer clicked - this won't fire');
});
```

### 3. stopImmediatePropagation

```javascript
// Stop other handlers on same element
element.addEventListener('click', (event) => {
  event.stopImmediatePropagation();
  console.log('First handler');
});

element.addEventListener('click', () => {
  console.log('Second handler - won't fire');
});
```

### 4. dispatchEvent

```javascript
// Programmatically trigger event
const button = document.querySelector('#myButton');
const clickEvent = new MouseEvent('click', {
  bubbles: true,
  cancelable: true
});

button.dispatchEvent(clickEvent);
```

## Creating Custom Events

### 1. Basic Custom Event

```javascript
// Create custom event
const userLogin = new CustomEvent('userLogin', {
  detail: {
    userId: 123,
    username: 'alice'
  }
});

// Dispatch event
document.dispatchEvent(userLogin);

// Listen for event
document.addEventListener('userLogin', (e) => {
  console.log('User logged in:', e.detail.username);
});
```

### 2. Custom Event with Options

```javascript
const notification = new CustomEvent('notification', {
  bubbles: true,      // Bubble up DOM
  cancelable: true,   // Can be cancelled
  composed: true,     // Cross shadow DOM
  detail: {
    type: 'success',
    message: 'Operation completed'
  }
});

element.dispatchEvent(notification);
```

### 3. Event Emitter Pattern

```javascript
class EventEmitter {
  constructor() {
    this.events = {};
  }

  on(event, callback) {
    if (!this.events[event]) {
      this.events[event] = [];
    }
    this.events[event].push(callback);
    return () => this.off(event, callback);
  }

  off(event, callback) {
    if (!this.events[event]) return;
    this.events[event] = this.events[event].filter(cb => cb !== callback);
  }

  emit(event, ...args) {
    if (!this.events[event]) return;
    this.events[event].forEach(callback => callback(...args));
  }

  once(event, callback) {
    const wrapper = (...args) => {
      callback(...args);
      this.off(event, wrapper);
    };
    return this.on(event, wrapper);
  }
}

const emitter = new EventEmitter();
emitter.on('data', (data) => console.log('Data:', data));
emitter.emit('data', { value: 42 });
```

## Common Event Types

### 1. Mouse Events

```javascript
element.addEventListener('click', handler);        // Click
element.addEventListener('dblclick', handler);      // Double click
element.addEventListener('mousedown', handler);     // Button pressed
element.addEventListener('mouseup', handler);       // Button released
element.addEventListener('mouseenter', handler);    // Enter element
element.addEventListener('mouseleave', handler);    // Leave element
element.addEventListener('mousemove', handler);     // Mouse move
element.addEventListener('mouseover', handler);     // Mouse over
element.addEventListener('mouseout', handler);      // Mouse out
```

### 2. Keyboard Events

```javascript
document.addEventListener('keydown', handler);      // Key pressed
document.addEventListener('keyup', handler);        // Key released
document.addEventListener('keypress', handler);     // Key press (deprecated)
```

### 3. Form Events

```javascript
input.addEventListener('input', handler);           // Input value change
input.addEventListener('change', handler);          // Value changed (on blur)
input.addEventListener('focus', handler);           // Element focused
input.addEventListener('blur', handler);            // Element lost focus
form.addEventListener('submit', handler);           // Form submitted
```

### 4. Window Events

```javascript
window.addEventListener('load', handler);           // Page fully loaded
document.addEventListener('DOMContentLoaded', handler); // DOM ready
window.addEventListener('resize', handler);         // Window resized
window.addEventListener('scroll', handler);         // Page scrolled
window.addEventListener('beforeunload', handler);   // Before leaving page
```

### 5. Clipboard Events

```javascript
element.addEventListener('copy', handler);          // Copy
element.addEventListener('cut', handler);           // Cut
element.addEventListener('paste', handler);         // Paste
```

### 6. Drag Events

```javascript
draggable.addEventListener('dragstart', handler);   // Drag started
draggable.addEventListener('drag', handler);        // Dragging
draggable.addEventListener('dragend', handler);     // Drag ended
dropzone.addEventListener('dragenter', handler);    // Entered dropzone
dropzone.addEventListener('dragover', handler);     // Over dropzone
dropzone.addEventListener('dragleave', handler);    // Left dropzone
dropzone.addEventListener('drop', handler);         // Dropped
```

## Event Delegation

### 1. Basic Delegation

```javascript
// Instead of adding listener to each item
const list = document.querySelector('#list');

list.addEventListener('click', (e) => {
  const item = e.target.closest('li');
  if (item) {
    console.log('Clicked:', item.textContent);
  }
});
```

### 2. Delegation with Data Attributes

```javascript
// HTML: <div id="container">
//   <button data-action="delete">Delete</button>
//   <button data-action="edit">Edit</button>
// </div>

const container = document.querySelector('#container');

container.addEventListener('click', (e) => {
  const button = e.target.closest('[data-action]');
  if (button) {
    const action = button.dataset.action;
    console.log('Action:', action);
  }
});
```

## Common Use Cases

### 1. Event Bus for Communication

```javascript
class EventBus {
  constructor() {
    this.events = {};
  }

  on(event, callback) {
    if (!this.events[event]) {
      this.events[event] = [];
    }
    this.events[event].push(callback);
    return () => this.off(event, callback);
  }

  off(event, callback) {
    if (!this.events[event]) return;
    this.events[event] = this.events[event].filter(cb => cb !== callback);
  }

  emit(event, data) {
    if (!this.events[event]) return;
    this.events[event].forEach(callback => callback(data));
  }
}

// Usage
const bus = new EventBus();
bus.on('user:login', (user) => console.log('User logged in:', user));
bus.emit('user:login', { name: 'Alice' });
```

### 2. Once Event Listener

```javascript
function once(element, event, callback) {
  const handler = (e) => {
    callback(e);
    element.removeEventListener(event, handler);
  };
  element.addEventListener(event, handler);
}

once(button, 'click', () => {
  console.log('Button clicked once');
});
```

### 3. Throttled Events

```javascript
function throttle(func, limit) {
  let inThrottle;
  return function(...args) {
    if (!inThrottle) {
      func.apply(this, args);
      inThrottle = true;
      setTimeout(() => inThrottle = false, limit);
    }
  };
}

window.addEventListener('scroll', throttle(() => {
  console.log('Scroll event throttled');
}, 100));
```

## Common Mistakes

### 1. Not Removing Event Listeners

```javascript
// BAD: Memory leak
function setup() {
  const button = document.querySelector('#btn');
  button.addEventListener('click', () => console.log('clicked'));
}

// BETTER: Clean up
function setup() {
  const button = document.querySelector('#btn');
  const handler = () => console.log('clicked');
  button.addEventListener('click', handler);
  return () => button.removeEventListener('click', handler);
}
```

### 2. Using Wrong Event Type

```javascript
// BAD: Using keypress (deprecated)
input.addEventListener('keypress', handler);

// BETTER: Use keydown
input.addEventListener('keydown', handler);
```

### 3. Not Preventing Default

```javascript
// BAD: Form submits
form.addEventListener('submit', () => {
  processForm();
});

// BETTER: Prevent default
form.addEventListener('submit', (e) => {
  e.preventDefault();
  processForm();
});
```

## Quick Revision Summary

- Events follow capturing → target → bubbling phases
- `preventDefault()` stops default browser behavior
- `stopPropagation()` stops event from bubbling
- Event delegation improves performance for dynamic content
- Custom events enable component communication
- Always clean up event listeners to prevent memory leaks
- Use `once: true` option for one-time listeners

## Related Topics

- [[Event-Handling]] - Adding event listeners
- [[DOM-Manipulation]] - Working with DOM elements
- [[Mouse-Events]] - Mouse event details
- [[Keyboard-Events]] - Keyboard event details
- [[Custom-Events]] - Creating custom events
- [[Event-Delegation]] - Efficient event handling
- [[Event-Bus]] - Component communication
