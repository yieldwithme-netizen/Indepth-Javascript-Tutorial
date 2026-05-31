# The Document Object (DOM)

## Definition

The `document` object is the entry point to the DOM (Document Object Model), representing the HTML document as a tree of objects. It provides methods to manipulate the content, structure, and styling of web pages.

## Accessing the Document Object

```javascript
// document is a global object in browsers
console.log(document.title);       // Page title
console.log(document.URL);         // Current URL
console.log(document.domain);      // Domain name
console.log(document.contentType); // MIME type
```

## Selecting Elements

### By ID

```javascript
const header = document.getElementById("header");
console.log(header.textContent);
```

### By Class Name

```javascript
const items = document.getElementsByClassName("item");
// Returns HTMLCollection (live)
Array.from(items).forEach(item => {
  console.log(item.textContent);
});
```

### By Tag Name

```javascript
const paragraphs = document.getElementsByTagName("p");
// Returns HTMLCollection
```

### CSS Selectors

```javascript
// First match
const firstBtn = document.querySelector(".btn");

// All matches
const allBtns = document.querySelectorAll(".btn");
// Returns NodeList (static)

allBtns.forEach(btn => {
  btn.addEventListener("click", handleClick);
});
```

## Creating and Modifying Elements

### Creating Elements

```javascript
// Create element
const div = document.createElement("div");
div.id = "new-div";
div.className = "container";
div.textContent = "Hello World";

// Create with innerHTML
const card = document.createElement("div");
card.innerHTML = `
  <h2>Title</h2>
  <p>Content</p>
  <button>Click me</button>
`;
```

### Modifying Elements

```javascript
const element = document.querySelector(".target");

// Text content
element.textContent = "New text";
element.innerText = "Visible text";

// HTML content
element.innerHTML = "<strong>Bold</strong>";

// Attributes
element.setAttribute("data-id", "123");
element.getAttribute("data-id");
element.removeAttribute("data-id");

// Classes
element.classList.add("active");
element.classList.remove("active");
element.classList.toggle("active");
element.classList.contains("active");

// Styles
element.style.color = "red";
element.style.backgroundColor = "blue";
```

### Appending Elements

```javascript
const parent = document.getElementById("parent");
const child = document.createElement("div");

// Methods
parent.appendChild(child);          // Add to end
parent.prepend(child);              // Add to start
parent.insertBefore(child, refNode); // Before reference
parent.append(child, "text", node);  // Multiple items

// Remove
parent.removeChild(child);
child.remove(); // Modern way
```

## Event Handling

```javascript
// addEventListener
const button = document.querySelector("button");

button.addEventListener("click", function(event) {
  console.log("Button clicked!");
  console.log(event.target);
  console.log(event.type);
});

// Remove event listener
function handleClick() {
  console.log("Clicked!");
}

button.addEventListener("click", handleClick);
button.removeEventListener("click", handleClick);

// Event object
document.addEventListener("click", (e) => {
  e.preventDefault();    // Prevent default action
  e.stopPropagation();   // Stop event bubbling
  console.log(e.clientX, e.clientY); // Mouse position
});
```

## DOM Traversal

```javascript
const element = document.querySelector(".node");

// Parent
element.parentNode;
element.parentElement;
element.closest(".ancestor"); // Nearest ancestor matching selector

// Children
element.children;            // HTMLCollection of elements
element.childNodes;          // NodeList including text nodes
element.firstElementChild;
element.lastElementChild;

// Siblings
element.nextElementSibling;
element.previousElementSibling;
```

## Common Use Cases

- Dynamic content updates
- Form validation
- Animations and transitions
- Single Page Applications
- Interactive user interfaces
- DOM manipulation libraries (jQuery, React)

## Common Mistakes

1. **Performance issues**: Too many DOM manipulations

```javascript
// Bad - multiple reflows
for (let i = 0; i < 100; i++) {
  list.innerHTML += `<li>Item ${i}</li>`;
}

// Good - batch update
const fragment = document.createDocumentFragment();
for (let i = 0; i < 100; i++) {
  const li = document.createElement("li");
  li.textContent = `Item ${i}`;
  fragment.appendChild(li);
}
list.appendChild(fragment);
```

2. **Forgetting to wait for DOM**: Scripts run before DOM is ready

```javascript
// Use DOMContentLoaded
document.addEventListener("DOMContentLoaded", () => {
  // DOM is ready
});

// Or place script at end of body
```

3. **Confusing HTMLCollection and NodeList**: NodeList has `forEach`, HTMLCollection doesn't

## Related Topics

- [[DOM-Manipulation]] - Modifying the DOM
- [[Events]] - Event handling and delegation
- [[DOM-Ready]] - Waiting for DOM to load
- [[Virtual-DOM]] - Efficient DOM updates
- [[jQuery]] - DOM manipulation library
- [[Web-APIs]] - Browser APIs

## Quick Revision Summary

| Method | Returns | Type |
|--------|---------|------|
| `getElementById()` | Single element | Element |
| `querySelector()` | First match | Element |
| `querySelectorAll()` | All matches | NodeList |
| `getElementsByClassName()` | All matches | HTMLCollection |
| `createElement()` | New element | Element |
| `appendChild()` | Added element | Element |
