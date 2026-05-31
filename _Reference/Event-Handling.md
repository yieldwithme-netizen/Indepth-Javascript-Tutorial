# Event Handling in JavaScript

## Definition

Event handling is the mechanism that allows JavaScript to respond to user interactions and browser events. Events are actions or occurrences that happen in the browser, such as clicks, key presses, mouse movements, or page loads. Event handling involves listening for these events and executing code when they occur.

## Adding Event Listeners

### 1. addEventListener

```javascript
const button = document.querySelector('#myButton');

button.addEventListener('click', function(event) {
  console.log('Button clicked!');
  console.log('Event type:', event.type);
});
```

### 2. Multiple Event Listeners

```javascript
const element = document.querySelector('#myElement');

// Add multiple listeners for same event
element.addEventListener('click', handler1);
element.addEventListener('click', handler2);

// Remove specific listener
element.removeEventListener('click', handler1);
```

### 3. Event Options

```javascript
const element = document.querySelector('#myElement');

element.addEventListener('click', handler, {
  once: true,      // Remove after first execution
  capture: true,   // Use capture phase
  passive: true,   // Never calls preventDefault()
  signal: controller.signal  // AbortController signal
});
```

## The Event Object

### 1. Common Properties

```javascript
button.addEventListener('click', (event) => {
  // Event type
  console.log(event.type); // "click"

  // Target element
  console.log(event.target); // The clicked element

  // Current target (element with listener)
  console.log(event.currentTarget); // Same as 'this'

  // Mouse position
  console.log(event.clientX, event.clientY);

  // Timestamp
  console.log(event.timeStamp);

  // Default prevented
  console.log(event.defaultPrevented);
});
```

### 2. Preventing Default Behavior

```javascript
// Prevent form submission
const form = document.querySelector('form');
form.addEventListener('submit', (event) => {
  event.preventDefault();
  console.log('Form submission prevented');
});

// Prevent link navigation
const link = document.querySelector('a');
link.addEventListener('click', (event) => {
  event.preventDefault();
  console.log('Link click prevented');
});
```

### 3. Stopping Propagation

```javascript
// Stop event from bubbling up
const inner = document.querySelector('#inner');
const outer = document.querySelector('#outer');

inner.addEventListener('click', (event) => {
  console.log('Inner clicked');
  event.stopPropagation(); // Prevents outer from receiving click
});

outer.addEventListener('click', () => {
  console.log('Outer clicked'); // Won't fire if inner stops propagation
});
```

## Event Types

### 1. Mouse Events

```javascript
const element = document.querySelector('#myElement');

element.addEventListener('click', (e) => {
  console.log('Clicked at:', e.clientX, e.clientY);
});

element.addEventListener('dblclick', () => {
  console.log('Double clicked');
});

element.addEventListener('mousedown', () => {
  console.log('Mouse button pressed');
});

element.addEventListener('mouseup', () => {
  console.log('Mouse button released');
});

element.addEventListener('mouseenter', () => {
  console.log('Mouse entered element');
});

element.addEventListener('mouseleave', () => {
  console.log('Mouse left element');
});

element.addEventListener('mousemove', (e) => {
  console.log('Mouse moving at:', e.clientX, e.clientY);
});
```

### 2. Keyboard Events

```javascript
const input = document.querySelector('input');

input.addEventListener('keydown', (e) => {
  console.log('Key pressed:', e.key);
  console.log('Key code:', e.code);
  console.log('Alt key:', e.altKey);
  console.log('Ctrl key:', e.ctrlKey);
  console.log('Shift key:', e.shiftKey);

  // Prevent default for specific keys
  if (e.key === 'Enter') {
    e.preventDefault();
    submitForm();
  }
});

input.addEventListener('keyup', (e) => {
  console.log('Key released:', e.key);
});

input.addEventListener('keypress', (e) => {
  console.log('Key pressed (deprecated)');
});
```

### 3. Form Events

```javascript
const input = document.querySelector('input');
const select = document.querySelector('select');
const textarea = document.querySelector('textarea');

// Input event (fires on every change)
input.addEventListener('input', (e) => {
  console.log('Input value:', e.target.value);
});

// Change event (fires when loses focus)
input.addEventListener('change', (e) => {
  console.log('Changed to:', e.target.value);
});

// Focus events
input.addEventListener('focus', () => {
  console.log('Input focused');
});

input.addEventListener('blur', () => {
  console.log('Input lost focus');
});

// Submit event
const form = document.querySelector('form');
form.addEventListener('submit', (e) => {
  e.preventDefault();
  const formData = new FormData(form);
  console.log('Form submitted:', Object.fromEntries(formData));
});
```

### 4. Window Events

```javascript
// Window load
window.addEventListener('load', () => {
  console.log('Page fully loaded');
});

// DOMContentLoaded
document.addEventListener('DOMContentLoaded', () => {
  console.log('DOM ready');
});

// Resize
window.addEventListener('resize', () => {
  console.log('Window resized:', window.innerWidth, window.innerHeight);
});

// Scroll
window.addEventListener('scroll', () => {
  console.log('Scrolled to:', window.scrollY);
});

// Before unload (warn user before leaving)
window.addEventListener('beforeunload', (e) => {
  if (hasUnsavedChanges) {
    e.preventDefault();
    e.returnValue = '';
  }
});
```

### 5. Touch Events (Mobile)

```javascript
const element = document.querySelector('#touchArea');

element.addEventListener('touchstart', (e) => {
  console.log('Touch started');
  console.log('Touch point:', e.touches[0].clientX, e.touches[0].clientY);
});

element.addEventListener('touchmove', (e) => {
  console.log('Touch moving');
  e.preventDefault(); // Prevent scrolling
});

element.addEventListener('touchend', (e) => {
  console.log('Touch ended');
});
```

## Event Delegation

### 1. Basic Event Delegation

```javascript
// Instead of adding listener to each item
const list = document.querySelector('#list');

list.addEventListener('click', (e) => {
  // Check if clicked element is a list item
  if (e.target.tagName === 'LI') {
    console.log('Clicked:', e.target.textContent);
  }
});
```

### 2. Delegation with Data Attributes

```javascript
// HTML: <ul id="list">
//   <li data-id="1">Item 1</li>
//   <li data-id="2">Item 2</li>
// </ul>

const list = document.querySelector('#list');

list.addEventListener('click', (e) => {
  const item = e.target.closest('li');
  if (item) {
    const id = item.dataset.id;
    console.log('Clicked item:', id);
  }
});
```

### 3. Dynamic Content Delegation

```javascript
// Works for dynamically added elements
const container = document.querySelector('#container');

container.addEventListener('click', (e) => {
  const button = e.target.closest('.delete-btn');
  if (button) {
    const card = button.closest('.card');
    card.remove();
  }
});

// Add new cards dynamically - still works!
function addCard(title) {
  const card = document.createElement('div');
  card.className = 'card';
  card.innerHTML = `
    <h3>${title}</h3>
    <button class="delete-btn">Delete</button>
  `;
  container.appendChild(card);
}
```

## Custom Events

### 1. Creating Custom Events

```javascript
// Create event
const customEvent = new CustomEvent('userLogin', {
  detail: {
    userId: 123,
    username: 'alice'
  }
});

// Dispatch event
document.dispatchEvent(customEvent);

// Listen for event
document.addEventListener('userLogin', (e) => {
  console.log('User logged in:', e.detail.username);
});
```

### 2. Event Emitter Pattern

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

const bus = new EventBus();
const unsub = bus.on('message', (data) => console.log(data));
bus.emit('message', 'Hello!'); // "Hello!"
unsub();
```

## Common Use Cases

### 1. Form Validation

```javascript
const form = document.querySelector('#signupForm');
const emailInput = form.querySelector('#email');

emailInput.addEventListener('blur', () => {
  const email = emailInput.value;
  const isValid = /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);

  if (!isValid) {
    emailInput.classList.add('error');
    showError('Please enter a valid email');
  } else {
    emailInput.classList.remove('error');
    hideError();
  }
});
```

### 2. Drag and Drop

```javascript
const draggable = document.querySelector('.draggable');
const dropzone = document.querySelector('.dropzone');

draggable.addEventListener('dragstart', (e) => {
  e.dataTransfer.setData('text/plain', e.target.id);
});

dropzone.addEventListener('dragover', (e) => {
  e.preventDefault();
  dropzone.classList.add('drag-over');
});

dropzone.addEventListener('drop', (e) => {
  e.preventDefault();
  const id = e.dataTransfer.getData('text/plain');
  const element = document.getElementById(id);
  dropzone.appendChild(element);
  dropzone.classList.remove('drag-over');
});
```

### 3. Infinite Scroll

```javascript
let loading = false;

window.addEventListener('scroll', () => {
  if (loading) return;

  const scrollPosition = window.innerHeight + window.scrollY;
  const documentHeight = document.documentElement.scrollHeight;

  if (scrollPosition >= documentHeight - 100) {
    loading = true;
    loadMoreContent().then(() => {
      loading = false;
    });
  }
});
```

### 4. Keyboard Shortcuts

```javascript
document.addEventListener('keydown', (e) => {
  // Ctrl/Cmd + S to save
  if ((e.ctrlKey || e.metaKey) && e.key === 's') {
    e.preventDefault();
    saveDocument();
  }

  // Ctrl/Cmd + Z to undo
  if ((e.ctrlKey || e.metaKey) && e.key === 'z') {
    e.preventDefault();
    undo();
  }
});
```

## Common Mistakes

### 1. Memory Leaks

```javascript
// BAD: Not removing listeners
function setup() {
  const button = document.querySelector('#btn');
  button.addEventListener('click', handler);
}

// BETTER: Clean up
function setup() {
  const button = document.querySelector('#btn');
  button.addEventListener('click', handler);

  return () => {
    button.removeEventListener('click', handler);
  };
}
```

### 2. Using the Wrong Event

```javascript
// BAD: Using keypress (deprecated)
input.addEventListener('keypress', handler);

// BETTER: Use keydown
input.addEventListener('keydown', handler);
```

### 3. Not Preventing Default

```javascript
// BAD: Form submits normally
form.addEventListener('submit', () => {
  processForm();
});

// BETTER: Prevent default first
form.addEventListener('submit', (e) => {
  e.preventDefault();
  processForm();
});
```

## Quick Revision Summary

- Use `addEventListener` to attach event handlers
- The event object contains event details and methods
- Use `preventDefault()` to stop default browser behavior
- Use `stopPropagation()` to prevent event bubbling
- Event delegation is efficient for dynamic content
- Clean up event listeners to prevent memory leaks
- Custom events enable component communication

## Related Topics

- [[Events]] - Event system and propagation
- [[DOM-Manipulation]] - Working with DOM elements
- [[Mouse-Events]] - Mouse event details
- [[Keyboard-Events]] - Keyboard event details
- [[Form-Events]] - Form-related events
- [[Touch-Events]] - Mobile touch events
