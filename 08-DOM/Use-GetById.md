# How to Use `getElementById()`

## Definition

The `getElementById()` method returns the element that has the ID attribute with the specified value. It's the most efficient way to select a single element by its ID.

**Syntax:**
```javascript
const element = document.getElementById("id");
```

**Returns:**
- The Element object if found
- `null` if no element with that ID exists

## Code Examples

### Basic Usage
```javascript
// HTML: <div id="content">Hello World</div>

const content = document.getElementById("content");
console.log(content);  // <div id="content">Hello World</div>
console.log(content.textContent);  // "Hello World"
```

### Common Operations

#### Change Text Content
```javascript
const heading = document.getElementById("main-heading");
heading.textContent = "New Heading Text";
```

#### Change HTML Content
```javascript
const container = document.getElementById("container");
container.innerHTML = "<p>New paragraph</p><p>Another paragraph</p>";
```

#### Change Styles
```javascript
const box = document.getElementById("box");
box.style.backgroundColor = "blue";
box.style.color = "white";
box.style.padding = "20px";
```

#### Add/Remove Classes
```javascript
const element = document.getElementById("my-element");
element.classList.add("active");
element.classList.remove("hidden");
element.classList.toggle("visible");
```

#### Get/Set Attributes
```javascript
const link = document.getElementById("my-link");
console.log(link.getAttribute("href"));  // "/page"
link.setAttribute("href", "/new-page");
link.setAttribute("target", "_blank");
```

### Practical Examples

#### Form Validation
```javascript
function validateForm() {
    const name = document.getElementById("name");
    const email = document.getElementById("email");
    const error = document.getElementById("error-message");

    if (!name.value.trim()) {
        error.textContent = "Name is required";
        name.focus();
        return false;
    }

    if (!email.value.includes("@")) {
        error.textContent = "Invalid email";
        email.focus();
        return false;
    }

    error.textContent = "";
    return true;
}
```

#### Dynamic Content Loading
```javascript
function loadContent(url) {
    const container = document.getElementById("content");
    const loader = document.getElementById("loader");

    loader.style.display = "block";

    fetch(url)
        .then(response => response.text())
        .then(html => {
            container.innerHTML = html;
            loader.style.display = "none";
        })
        .catch(error => {
            container.innerHTML = "<p>Error loading content</p>";
            loader.style.display = "none";
        });
}
```

#### Toggle Visibility
```javascript
function toggleSection(sectionId) {
    const section = document.getElementById(sectionId);
    const icon = document.getElementById(sectionId + "-icon");

    if (section.style.display === "none") {
        section.style.display = "block";
        icon.textContent = "▼";
    } else {
        section.style.display = "none";
        icon.textContent = "▶";
    }
}
```

#### Update Counter
```javascript
function updateCounter() {
    const counter = document.getElementById("counter");
    let count = parseInt(counter.textContent) || 0;
    counter.textContent = count + 1;
}

document.getElementById("increment-btn").addEventListener("click", updateCounter);
```

#### Create Element and Append
```javascript
function addListItem(text) {
    const list = document.getElementById("my-list");
    const newItem = document.createElement("li");
    newItem.textContent = text;
    newItem.id = "item-" + Date.now();
    list.appendChild(newItem);
}
```

## Common Use Cases

1. **Form handling** and validation
2. **Dynamic content updates**
3. **UI state management**
4. **Event handling**
5. **DOM manipulation**

## Common Mistakes

1. **Not checking for null**: Always verify element exists before manipulating
2. **Using duplicate IDs**: IDs must be unique in HTML
3. **Calling before DOM ready**: Wait for DOMContentLoaded
4. **Using for multiple elements**: Use `querySelectorAll` instead

## Related Topics

- [[Select-Elements]]
- [[Change-HTML]]
- [[Add-Event-Listeners]]
- [[Form-Handling]]

## Quick Revision Summary

| Property/Method | Description |
|-----------------|-------------|
| `getElementById("id")` | Select element by ID |
| `.textContent` | Get/set text content |
| `.innerHTML` | Get/set HTML content |
| `.style` | Access inline styles |
| `.classList` | Manage CSS classes |
| `.getAttribute()` | Get attribute value |
| `.setAttribute()` | Set attribute value |
| Returns `null` | If element not found |
