# DOM (Document Object Model)

## Definition
The DOM (Document Object Model) is a programming interface for HTML documents. It represents the page as a tree of nodes that can be manipulated to change the document structure, style, and content.

## Selecting Elements

### By ID
```javascript
const header = document.getElementById('header');
console.log(header);
```

### By Class Name
```javascript
const items = document.getElementsByClassName('list-item');
console.log(items); // HTMLCollection
```

### By Tag Name
```javascript
const paragraphs = document.getElementsByTagName('p');
console.log(paragraphs); // HTMLCollection
```

### Query Selector (Modern)
```javascript
// Single element
const firstButton = document.querySelector('.btn-primary');

// Multiple elements
const allButtons = document.querySelectorAll('.btn');

// Complex selectors
const特定Item = document.querySelector('#menu > .item:first-child');
```

## Modifying Elements

### Changing Content
```javascript
// Text content
element.textContent = 'New text';

// HTML content
element.innerHTML = '<strong>Bold text</strong>';

// Safe HTML insertion
element.textContent = userInput; // Prevents XSS
```

### Changing Attributes
```javascript
// Set attribute
element.setAttribute('class', 'active');

// Get attribute
const href = element.getAttribute('href');

// Remove attribute
element.removeAttribute('disabled');

// Data attributes
const id = element.dataset.userId;
```

### Changing Styles
```javascript
// Inline styles
element.style.color = 'red';
element.style.backgroundColor = '#f0f0f0';
element.style.fontSize = '16px';

// CSS classes
element.classList.add('active');
element.classList.remove('hidden');
element.classList.toggle('visible');
element.classList.contains('active'); // check
```

## Creating & Removing Elements

### Creating Elements
```javascript
// Create element
const newDiv = document.createElement('div');
newDiv.className = 'card';
newDiv.textContent = 'Hello World';

// Add to DOM
document.body.appendChild(newDiv);

// Insert before specific element
parent.insertBefore(newDiv, referenceElement);

// Insert at specific position
parent.append(child1, child2, child3);
```

### Removing Elements
```javascript
// Remove element
element.remove();

// Remove child
parent.removeChild(child);
```

## Traversing the DOM

### Parent, Children, Siblings
```javascript
// Parent
const parent = element.parentElement;
const parentDirect = element.parentNode;

// Children
const children = element.children; // HTMLCollection
const firstChild = element.firstElementChild;
const lastChild = element.lastElementChild;

// Siblings
const next = element.nextElementSibling;
const previous = element.previousElementSibling;
```

### Walking the Tree
```javascript
function traverseDOM(node, depth = 0) {
  console.log('  '.repeat(depth) + node.nodeName);
  
  for (const child of node.children) {
    traverseDOM(child, depth + 1);
  }
}

traverseDOM(document.body);
```

## Common Use Cases
- Dynamic content loading
- Form validation
- Interactive UI components
- Single Page Applications (SPAs)
- DOM-based games
- Real-time updates

## Common Mistakes
- Querying DOM too frequently (cache selections)
- Modifying DOM directly in loops (causes reflows)
- Not using document fragments for batch inserts
- Forgetting to remove event listeners
- Mixing innerHTML with user input (XSS risk)

## Quick Revision Summary
- `querySelector()` / `querySelectorAll()` - modern selection
- `createElement()` / `appendChild()` - create elements
- `textContent` / `innerHTML` - change content
- `style` / `classList` - modify appearance
- `addEventListener()` - handle events
- Use document fragments for performance

## Related Topics
- [[Events]]
- [[Selectors]]
- [[CSS-in-JS]]
- [[Virtual-DOM]]
- [[Frontend-Frameworks]]
