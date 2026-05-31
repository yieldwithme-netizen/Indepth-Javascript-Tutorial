# appendChild

The `appendChild()` method adds a node to the end of the list of children of a specified parent node. It is a fundamental DOM manipulation method for dynamically building web pages.

## Definition

`appendChild()` appends a child element, text node, or any DOM node to a parent element. It returns the appended child node. If the node already exists in the DOM, it will be moved from its current position.

**Syntax:**
```javascript
parentNode.appendChild(childNode);
```

**Parameters:**
- `childNode`: The node to append (Element, Text node, or DocumentFragment)

**Returns:** The appended child node

## Basic Examples

```javascript
// Create and append a new element
const container = document.querySelector('#app');

const newDiv = document.createElement('div');
newDiv.textContent = 'Hello World';
container.appendChild(newDiv);

// Append existing element (moves it)
const existing = document.querySelector('#moveMe');
const target = document.querySelector('#target');
target.appendChild(existing); // existing element moves to target

// Append text node
const textNode = document.createTextNode('Some text');
container.appendChild(textNode);
```

## Common Use Cases

```javascript
// Dynamic list items
function addListItem(text) {
    const list = document.querySelector('#myList');
    const li = document.createElement('li');
    li.textContent = text;
    list.appendChild(li);
}

// Building a card component
function createCard(title, description) {
    const card = document.createElement('div');
    card.className = 'card';
    
    const heading = document.createElement('h3');
    heading.textContent = title;
    card.appendChild(heading);
    
    const para = document.createElement('p');
    para.textContent = description;
    card.appendChild(para);
    
    return card;
}

// Appending multiple elements with DocumentFragment
function addMultipleItems(items) {
    const list = document.querySelector('#myList');
    const fragment = document.createDocumentFragment();
    
    items.forEach(item => {
        const li = document.createElement('li');
        li.textContent = item;
        fragment.appendChild(li);
    });
    
    list.appendChild(fragment); // Single DOM update
}
```

## Differences from Related Methods

```javascript
// appendChild - adds to end
parent.appendChild(child);

// append - more flexible, accepts multiple arguments and strings
parent.append(child1, child2, 'text');

// prepend - adds to beginning
parent.prepend(child);

// insertBefore - insert before specific node
parent.insertBefore(newNode, referenceNode);

// replaceChild - replace existing child
parent.replaceChild(newChild, oldChild);
```

## Using with Different Node Types

```javascript
// Append element
const div = document.createElement('div');
parent.appendChild(div);

// Append text node
const text = document.createTextNode('Hello');
parent.appendChild(text);

// Append HTML string (not directly - must parse first)
const htmlString = '<p>Paragraph</p>';
const temp = document.createElement('div');
temp.innerHTML = htmlString;
parent.appendChild(temp.firstChild);

// Append DocumentFragment
const fragment = document.createDocumentFragment();
fragment.appendChild(document.createElement('li'));
parent.appendChild(fragment);
```

## Common Use Cases

- Building dynamic forms
- Creating list items from data
- Adding notifications or alerts
- Inserting content loaded from APIs
- Building component-based UIs

## Common Mistakes

1. **Appending existing elements** - The element moves, doesn't clone. Use `cloneNode(true)` if you want a copy
2. **Appending to wrong parent** - Always verify the parent node exists
3. **Performance issues with multiple appends** - Use DocumentFragment for batch insertions
4. **Not handling null returns** - `appendChild` can fail silently if parent is invalid
5. **Appending removed elements** - Removed nodes still reference old parent

## Related Topics

- [[DOM Manipulation]]
- [[createElement]]
- [[DocumentFragment]]
- [[insertAdjacentHTML]]
- [[DOM Traversal]]

## Quick Revision

| Method | Description |
|--------|-------------|
| `appendChild(node)` | Add child to end of parent |
| `append(nodes)` | Add multiple nodes/strings |
| `prepend(node)` | Add child to beginning |
| `insertBefore(new, ref)` | Insert before reference node |
| `replaceChild(new, old)` | Replace existing child |
| `cloneNode(deep)` | Clone element before appending |
