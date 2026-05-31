# createElement

The `document.createElement()` method creates a new HTML element node that you can then add to the DOM. It's fundamental for dynamic web page manipulation.

## Basic Usage

```javascript
// Create a new element
const div = document.createElement('div');

// Create with specific tag
const paragraph = document.createElement('p');
const button = document.createElement('button');
const image = document.createElement('img');
```

## Setting Properties

```javascript
const element = document.createElement('div');

// Set id
element.id = 'myDiv';

// Set classes
element.className = 'container active';

// Set multiple classes
element.classList.add('container', 'active');
element.classList.remove('hidden');
element.classList.toggle('visible');

// Set attributes
element.setAttribute('data-id', '123');
element.setAttribute('role', 'button');

// Set content
element.innerHTML = '<strong>Hello</strong> World';
element.textContent = 'Plain text content';

// Set style
element.style.color = 'blue';
element.style.backgroundColor = '#f0f0f0';
```

## Adding to DOM

```javascript
const parent = document.getElementById('parent');

// Append to end
parent.appendChild(element);

// Insert before specific child
parent.insertBefore(element, parent.firstChild);

// Insert adjacent HTML
parent.insertAdjacentHTML('beforeend', '<div>New content</div>');
parent.insertAdjacentElement('afterend', element);
```

## Creating Complete Elements

```javascript
function createCard(title, content, imageUrl) {
  const card = document.createElement('div');
  card.className = 'card';
  
  const img = document.createElement('img');
  img.src = imageUrl;
  img.alt = title;
  img.className = 'card-image';
  
  const cardContent = document.createElement('div');
  cardContent.className = 'card-content';
  
  const heading = document.createElement('h3');
  heading.textContent = title;
  
  const paragraph = document.createElement('p');
  paragraph.textContent = content;
  
  cardContent.appendChild(heading);
  cardContent.appendChild(paragraph);
  card.appendChild(img);
  card.appendChild(cardContent);
  
  return card;
}

// Usage
const myCard = createCard('Hello', 'This is a card', 'image.jpg');
document.body.appendChild(myCard);
```

## Using DocumentFragment

For performance when adding multiple elements:

```javascript
function createList(items) {
  const fragment = document.createDocumentFragment();
  
  items.forEach(item => {
    const li = document.createElement('li');
    li.textContent = item;
    fragment.appendChild(li);
  });
  
  return fragment;
}

// Single DOM update
const list = document.createElement('ul');
list.appendChild(createList(['Item 1', 'Item 2', 'Item 3']));
document.body.appendChild(list);
```

## Event Listeners

```javascript
const button = document.createElement('button');
button.textContent = 'Click me';
button.addEventListener('click', () => {
  alert('Button clicked!');
});

// Delegation pattern
document.addEventListener('click', (e) => {
  if (e.target.matches('.dynamic-button')) {
    console.log('Dynamic button clicked');
  }
});
```

## Common Use Cases

- Dynamic content loading
- Single Page Applications (SPA)
- Interactive UI components
- Real-time updates
- Form generation

## Common Mistakes

1. **Not appending to DOM** - Elements exist but aren't visible
2. **Memory leaks** - Not removing event listeners
3. **Overusing innerHTML** - Can cause XSS vulnerabilities
4. **Ignoring performance** - Use DocumentFragment for multiple elements
5. **Not setting attributes** - Missing required properties

## Related Topics

- [[DOM-Manipulation]]
- [[Event-Handling]]
- [[DOM-Manipulation]]
- [[DocumentFragment]]
- [[Virtual-DOM]]

## Quick Revision

| Method | Purpose |
|--------|---------|
| `createElement()` | Create new element |
| `appendChild()` | Add to parent |
| `insertBefore()` | Insert before child |
| `DocumentFragment` | Batch DOM updates |
| `classList` | Manage CSS classes |

`createElement()` is essential for dynamic web applications, enabling JavaScript to create and modify the DOM structure programmatically.