# What is DOM (Document Object Model)?

## Definition

[[DOM]] is a **tree-structured representation** of your HTML document. It's an API that lets [[JavaScript]] **read and manipulate** the content, structure, and styles of a web page.

## [[DOM]] Tree Structure

```
document
  └── html
        ├── head
        │     ├── title
        │     └── meta
        └── body
              ├── h1
              ├── p
              └── div
                    ├── span
                    └── button
```

## How Browser Creates [[DOM]]

```
HTML Code → Parser → DOM Tree → Rendering Engine → Pixels on Screen
```

## [[DOM]] Nodes

| Node Type | Example | Description |
|-----------|---------|-------------|
| Document | `[[document]]` | Root node |
| Element | `<div>`, `<p>` | HTML elements |
| Text | `"Hello"` | Text content |
| Attribute | `class="box"` | Element attributes |
| Comment | `<!-- comment -->` | HTML comments |

## [[DOM]] Properties

```javascript
// Every element has these properties
element.parentNode      // Parent node
element.childNodes      // All child nodes
element.children        // Only element children
element.firstChild      // First child
element.lastChild       // Last child
element.nextSibling     // Next sibling
element.previousSibling // Previous sibling
```

## [[DOM]] Methods

```javascript
// Selecting elements
document.getElementById("id");
document.querySelector(".class");
document.querySelectorAll("p");

// Creating elements
document.createElement("div");
document.createTextNode("text");

// Modifying elements
element.appendChild(child);
element.removeChild(child);
element.replaceChild(new, old);
element.insertBefore(new, reference);
```

## [[DOM]] vs HTML

| HTML | [[DOM]] |
|------|-----|
| Static text file | Living JavaScript object |
| Source code | Parsed representation |
| Can't be changed by JS directly | Can be modified by JS |

## Quick Revision

- [[DOM]] = tree representation of HTML
- Created by browser when page loads
- [[JavaScript]] uses [[DOM]] to manipulate page
- Nodes: Document, Element, Text, Attribute
- Methods: [[querySelector]], [[createElement]], [[appendChild]]

---

## Related Topics

- [[What-is-JavaScript]] - JS basics
- [[What-is-Browser-Engine]] - How browsers work
- [[Select-Elements]] - Selecting DOM elements
- [[Change-HTML]] - Modifying content
- [[Create-Elements]] - Creating new elements