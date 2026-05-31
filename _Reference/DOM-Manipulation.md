# DOM Manipulation

## Definition

DOM (Document Object Model) manipulation is the process of modifying the structure, content, and style of a web page using JavaScript. The DOM is a tree-like representation of an HTML document that JavaScript can interact with to dynamically update the page without reloading.

## The DOM Tree

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Page</title>
  </head>
  <body>
    <div id="app">
      <h1 class="title">Hello</h1>
      <p>World</p>
    </div>
  </body>
</html>
```

## Selecting Elements

### 1. getElementById

```javascript
const element = document.getElementById('app');
console.log(element); // <div id="app">...</div>
```

### 2. querySelector

```javascript
// First matching element
const title = document.querySelector('.title');
const firstParagraph = document.querySelector('p');
const nested = document.querySelector('#app > h1');
```

### 3. querySelectorAll

```javascript
// All matching elements (NodeList)
const allParagraphs = document.querySelectorAll('p');
const allItems = document.querySelectorAll('.item');

// Convert to array for easier manipulation
const itemsArray = Array.from(allItems);
```

### 4. Other Selectors

```javascript
// By tag name
const divs = document.getElementsByTagName('div');

// By class name
const items = document.getElementsByClassName('item');

// By name attribute
const inputs = document.getElementsByName('email');
```

## Modifying Content

### 1. textContent

```javascript
const element = document.querySelector('.title');

// Get text
console.log(element.textContent); // "Hello"

// Set text (removes HTML tags)
element.textContent = 'New Title';
```

### 2. innerHTML

```javascript
const container = document.getElementById('app');

// Get HTML
console.log(container.innerHTML);

// Set HTML (parses HTML tags)
container.innerHTML = `
  <h1>New Title</h1>
  <p>New paragraph</p>
`;

// Append HTML
container.innerHTML += '<p>Another paragraph</p>';
```

### 3. innerText

```javascript
// Similar to textContent but considers CSS visibility
const element = document.querySelector('.title');
console.log(element.innerText);
```

## Modifying Attributes

### 1. setAttribute / getAttribute

```javascript
const link = document.querySelector('a');

// Set attribute
link.setAttribute('href', 'https://example.com');
link.setAttribute('target', '_blank');

// Get attribute
const href = link.getAttribute('href');
```

### 2. Direct Property Access

```javascript
const img = document.querySelector('img');

// Set properties
img.src = 'image.jpg';
img.alt = 'Description';
img.width = 300;

// Get properties
console.log(img.src);
```

### 3. dataset (data-* attributes)

```javascript
// HTML: <div data-user-id="123" data-role="admin">
const element = document.querySelector('[data-user-id]');

// Access via dataset
console.log(element.dataset.userId); // "123"
console.log(element.dataset.role);   // "admin"

// Set dataset
element.dataset.status = 'active';
```

## Modifying Styles

### 1. style Property

```javascript
const element = document.querySelector('.box');

// Set individual styles
element.style.backgroundColor = 'blue';
element.style.color = 'white';
element.style.padding = '20px';
element.style.borderRadius = '8px';
```

### 2. cssText

```javascript
const element = document.querySelector('.box');

// Set multiple styles at once
element.cssText = `
  background-color: blue;
  color: white;
  padding: 20px;
  border-radius: 8px;
`;
```

### 3. classList

```javascript
const element = document.querySelector('.box');

// Add class
element.classList.add('active');
element.classList.add('highlight', 'visible');

// Remove class
element.classList.remove('active');

// Toggle class
element.classList.toggle('hidden');

// Check if class exists
if (element.classList.contains('active')) {
  console.log('Element is active');
}

// Replace class
element.classList.replace('old-class', 'new-class');
```

## Creating Elements

### 1. createElement

```javascript
// Create element
const div = document.createElement('div');
div.className = 'card';
div.id = 'card-1';

// Create nested elements
const title = document.createElement('h2');
title.textContent = 'Card Title';

const content = document.createElement('p');
content.textContent = 'Card content here';

// Assemble
div.appendChild(title);
div.appendChild(content);

// Add to DOM
document.body.appendChild(div);
```

### 2. insertAdjacentHTML

```javascript
const container = document.getElementById('app');

// Before beginning (inside, at top)
container.insertAdjacentHTML('afterbegin', '<p>First</p>');

// Before end (inside, at bottom)
container.insertAdjacentHTML('beforeend', '<p>Last</p>');

// Before element
container.insertAdjacentHTML('beforebegin', '<p>Before</p>');

// After element
container.insertAdjacentHTML('afterend', '<p>After</p>');
```

### 3. DocumentFragment

```javascript
// Create fragment (doesn't trigger reflow)
const fragment = document.createDocumentFragment();

// Add multiple elements
for (let i = 0; i < 100; i++) {
  const li = document.createElement('li');
  li.textContent = `Item ${i}`;
  fragment.appendChild(li);
}

// Append all at once (single reflow)
document.querySelector('ul').appendChild(fragment);
```

## Removing Elements

### 1. remove()

```javascript
const element = document.querySelector('.old-item');
element.remove();
```

### 2. removeChild

```javascript
const parent = document.getElementById('container');
const child = document.getElementById('item');

parent.removeChild(child);
```

## Replacing Elements

### 1. replaceChild

```javascript
const parent = document.getElementById('container');
const oldChild = document.getElementById('old-item');
const newChild = document.createElement('div');
newChild.textContent = 'New Item';

parent.replaceChild(newChild, oldChild);
```

### 2. replaceWith

```javascript
const element = document.querySelector('.old');
const newElement = document.createElement('div');
newElement.textContent = 'Replacement';

element.replaceWith(newElement);
```

## Cloning Elements

```javascript
const original = document.querySelector('.card');

// Shallow clone (no children)
const clone1 = original.cloneNode(false);

// Deep clone (with children)
const clone2 = original.cloneNode(true);

// Modify clone
clone2.id = 'card-clone';
clone2.querySelector('h2').textContent = 'Cloned Card';

document.body.appendChild(clone2);
```

## Traversing the DOM

### Parent Nodes

```javascript
const element = document.querySelector('.child');

// Direct parent
const parent = element.parentNode;
const parentElement = element.parentElement;

// Closest ancestor matching selector
const section = element.closest('section');
```

### Child Nodes

```javascript
const parent = document.getElementById('container');

// All children (includes text nodes)
const allNodes = parent.childNodes;

// Element children only
const children = parent.children;

// First/last child
const first = parent.firstElementChild;
const last = parent.lastElementChild;
```

### Sibling Nodes

```javascript
const element = document.querySelector('.item');

// Next/previous sibling
const next = element.nextElementSibling;
const prev = element.previousElementSibling;
```

## Common Use Cases

### Dynamic List

```javascript
function addListItem(text) {
  const ul = document.querySelector('#list');
  const li = document.createElement('li');
  li.textContent = text;

  // Add delete button
  const deleteBtn = document.createElement('button');
  deleteBtn.textContent = '×';
  deleteBtn.onclick = () => li.remove();
  li.appendChild(deleteBtn);

  ul.appendChild(li);
}
```

### Show/Hide Elements

```javascript
function toggleElement(selector) {
  const element = document.querySelector(selector);
  element.classList.toggle('hidden');
}

// CSS
// .hidden { display: none; }
```

### Form Handling

```javascript
const form = document.querySelector('#myForm');

form.addEventListener('submit', (e) => {
  e.preventDefault();

  const formData = new FormData(form);
  const data = Object.fromEntries(formData);

  console.log('Form data:', data);

  // Clear form
  form.reset();
});
```

### Template Rendering

```javascript
function renderList(items) {
  const container = document.getElementById('list');
  container.innerHTML = items.map(item => `
    <div class="item">
      <h3>${item.name}</h3>
      <p>${item.description}</p>
      <button onclick="deleteItem(${item.id})">Delete</button>
    </div>
  `).join('');
}
```

## Common Mistakes

### 1. Forgetting to Select Element

```javascript
// WRONG: element is null
const element = document.querySelector('.nonexistent');
element.style.color = 'red'; // Error!

// RIGHT: Check if element exists
const element = document.querySelector('.nonexistent');
if (element) {
  element.style.color = 'red';
}
```

### 2. Performance Issues with innerHTML

```javascript
// BAD: Triggers reflow for each addition
for (let i = 0; i < 1000; i++) {
  container.innerHTML += `<div>Item ${i}</div>`;
}

// BETTER: Build string first, then set once
const html = Array.from({ length: 1000 }, (_, i) =>
  `<div>Item ${i}</div>`
).join('');
container.innerHTML = html;

// BEST: Use DocumentFragment
```

### 3. Memory Leaks with Event Listeners

```javascript
// BAD: Leaks memory
function addClickHandler() {
  const button = document.querySelector('#btn');
  button.addEventListener('click', () => {
    console.log('clicked');
  });
}

// BETTER: Remove when done
function addClickHandler() {
  const button = document.querySelector('#btn');
  const handler = () => console.log('clicked');
  button.addEventListener('click', handler);

  return () => button.removeEventListener('click', handler);
}
```

## Quick Revision Summary

- **Select**: `getElementById`, `querySelector`, `querySelectorAll`
- **Modify Content**: `textContent`, `innerHTML`, `innerText`
- **Modify Attributes**: `setAttribute`, `getAttribute`, `dataset`
- **Modify Styles**: `style`, `classList`, `cssText`
- **Create**: `createElement`, `insertAdjacentHTML`, `DocumentFragment`
- **Remove**: `remove()`, `removeChild()`
- **Traverse**: `parentNode`, `children`, `nextElementSibling`

## Related Topics

- [[Event-Handling]] - Handling user interactions
- [[Events]] - Event system and propagation
- [[Selectors-API]] - CSS selector methods
- [[Performance-Optimization]] - Efficient DOM manipulation
- [[Virtual-DOM]] - React's virtual DOM concept
- [[Web-Components]] - Custom HTML elements
- [[Template-Literals]] - String templates for HTML
