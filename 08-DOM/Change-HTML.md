# How to Change HTML Content with `innerHTML`

## Definition

The `innerHTML` property gets or sets the HTML markup contained within an element. It allows you to read, insert, or replace the HTML content of an element.

**Syntax:**
```javascript
// Get HTML content
const html = element.innerHTML;

// Set HTML content
element.innerHTML = "<p>New content</p>";
```

## Code Examples

### Basic Usage
```javascript
// Get HTML content
const container = document.getElementById("container");
console.log(container.innerHTML);

// Set HTML content
container.innerHTML = "<h2>New Heading</h2><p>New paragraph</p>";
```

### Practical Examples

#### Dynamic Content Injection
```javascript
function loadProduct(product) {
    const productCard = document.getElementById("product-card");
    productCard.innerHTML = `
        <img src="${product.image}" alt="${product.name}">
        <h3>${product.name}</h3>
        <p class="price">$${product.price}</p>
        <p>${product.description}</p>
        <button onclick="addToCart('${product.id}')">Add to Cart</button>
    `;
}
```

#### Create Table from Data
```javascript
function renderTable(data) {
    const tableContainer = document.getElementById("table-container");

    let html = "<table><thead><tr>";
    html += "<th>Name</th><th>Email</th><th>Role</th>";
    html += "</tr></thead><tbody>";

    data.forEach(row => {
        html += "<tr>";
        html += `<td>${row.name}</td>`;
        html += `<td>${row.email}</td>`;
        html += `<td>${row.role}</td>`;
        html += "</tr>";
    });

    html += "</tbody></table>";
    tableContainer.innerHTML = html;
}

const users = [
    { name: "John", email: "john@example.com", role: "Admin" },
    { name: "Jane", email: "jane@example.com", role: "User" }
];

renderTable(users);
```

#### Build Navigation Menu
```javascript
function buildNav(items) {
    const nav = document.getElementById("main-nav");
    let html = "<ul>";

    items.forEach(item => {
        html += `
            <li>
                <a href="${item.url}" class="${item.active ? 'active' : ''}">
                    ${item.label}
                </a>
            </li>
        `;
    });

    html += "</ul>";
    nav.innerHTML = html;
}

const menuItems = [
    { label: "Home", url: "/", active: true },
    { label: "About", url: "/about", active: false },
    { label: "Contact", url: "/contact", active: false }
];

buildNav(menuItems);
```

#### Append Content (Without Removing Existing)
```javascript
function addMessage(message) {
    const chat = document.getElementById("chat");
    const newMessage = document.createElement("div");
    newMessage.className = "message";
    newMessage.innerHTML = `
        <span class="sender">${message.sender}:</span>
        <span class="text">${message.text}</span>
        <span class="time">${message.time}</span>
    `;
    chat.appendChild(newMessage);
    chat.scrollTop = chat.scrollHeight;
}
```

#### Update Score Display
```javascript
function updateScore(score) {
    const scoreDisplay = document.getElementById("score");
    scoreDisplay.innerHTML = `
        <div class="score-value">${score}</div>
        <div class="score-label">Points</div>
        <div class="score-rank ${getRankClass(score)}">
            ${getRank(score)}
        </div>
    `;
}

function getRank(score) {
    if (score >= 1000) return "Gold";
    if (score >= 500) return "Silver";
    return "Bronze";
}

function getRankClass(score) {
    if (score >= 1000) return "gold";
    if (score >= 500) return "silver";
    return "bronze";
}
```

#### Clear Content
```javascript
function clearContent() {
    document.getElementById("results").innerHTML = "";
    document.getElementById("error").innerHTML = "";
}

function showError(message) {
    document.getElementById("error").innerHTML = `
        <div class="alert alert-danger">
            <strong>Error:</strong> ${message}
        </div>
    `;
}
```

### Alternative Methods

#### Using `textContent` (Plain Text Only)
```javascript
// Safe for plain text - no HTML parsing
const element = document.getElementById("text-content");
element.textContent = "This is plain text <no> HTML</no>";
// Will display the tags as text, not render them
```

#### Using `insertAdjacentHTML()`
```javascript
const container = document.getElementById("container");

// Before end (like appendChild)
container.insertAdjacentHTML("beforeend", "<p>New item</p>");

// After begin (prepend)
container.insertAdjacentHTML("afterbegin", "<p>First item</p>");

// Before begin (insert before element)
container.insertAdjacentHTML("beforebegin", "<p>Before container</p>");

// After end (insert after element)
container.insertAdjacentHTML("afterend", "<p>After container</p>");
```

## Common Use Cases

1. **Dynamic page rendering**
2. **Single Page Applications (SPA)**
3. **Real-time content updates**
4. **Form building from data**
5. **Notification systems**

## Common Mistakes

1. **XSS vulnerabilities**: Never insert user input directly
2. **Performance issues**: Frequent updates cause reflows
3. **Losing event listeners**: Replacing content removes attached events
4. **Not escaping HTML**: Special characters need escaping

## Security Warning

```javascript
// NEVER do this with user input (XSS vulnerability)
element.innerHTML = userInput;

// ALWAYS sanitize user input
function sanitize(str) {
    const div = document.createElement("div");
    div.textContent = str;
    return div.innerHTML;
}

element.innerHTML = sanitize(userInput);
```

## Related Topics

- [[Use-GetById]]
- [[Select-Elements]]
- [[DOM-Manipulation]]
- [[Event-Handling]]

## Quick Revision Summary

| Property | Description |
|----------|-------------|
| `innerHTML` | Get/set HTML content |
| `textContent` | Get/set plain text (safer) |
| `insertAdjacentHTML()` | Insert HTML at specific position |
| Original content | Replaced when setting |
| Event listeners | Lost when replacing content |
| Security | Sanitize user input |
