# Changing Styles with the Style Property

## Definition

The `style` property allows you to directly modify inline CSS styles of an element. Each CSS property becomes a JavaScript property (using camelCase naming).

## Syntax

```javascript
element.style.property = "value";

// CSS Property → JS Property
// background-color → backgroundColor
// font-size → fontSize
// margin-top → marginTop
```

## Code Examples

### Basic Style Changes

```javascript
// Change color
document.getElementById("box").style.color = "red";

// Change background
element.style.backgroundColor = "#3498db";

// Change font size
element.style.fontSize = "24px";

// Change multiple styles
element.style.cssText = "color: blue; font-size: 16px; padding: 10px;";
```

### Setting Multiple Styles at Once

```javascript
// Using object assignment
Object.assign(element.style, {
    backgroundColor: "yellow",
    border: "1px solid black",
    borderRadius: "8px",
    padding: "15px"
});
```

### Reading Current Styles

```javascript
// Get computed style (returns actual rendered value)
let computedStyle = window.getComputedStyle(element);
let color = computedStyle.color;
let fontSize = computedStyle.fontSize;
```

### Dynamic Style Calculations

```javascript
// Increase font size
let currentSize = parseInt(element.style.fontSize) || 16;
element.style.fontSize = (currentSize + 2) + "px";

// Toggle visibility
element.style.display = element.style.display === "none" ? "block" : "none";
```

### CSS Custom Properties (Variables)

```javascript
// Set CSS variable
element.style.setProperty("--primary-color", "#3498db");

// Get CSS variable
let primary = getComputedStyle(element).getPropertyValue("--primary-color");
```

## Common Use Cases

1. **Animations**: Create smooth transitions
2. **Responsive design**: Adjust styles based on screen size
3. **Theme changes**: Modify colors, fonts dynamically
4. **Form validation**: Highlight invalid fields
5. **Progress bars**: Update width based on progress

## Common Mistakes

1. **Forgetting units** - Always add px, em, rem, % for numeric values
2. **Using kebab-case** - JavaScript uses camelCase (fontSize not font-size)
3. **Overusing inline styles** - Prefer CSS classes for reusable styles
4. **Not checking computed styles** - Use getComputedStyle() for reading

## Related Topics

- [[Manage-Classes]] - Better for reusable styles
- [[Change-Text]] - Update text content
- [[Create-Elements]] - Create elements with styles
- [[Add-Elements]] - Add styled elements to DOM

## Quick Revision

| Method | Use Case |
|--------|----------|
| element.style.prop | Set single style |
| cssText | Set multiple styles at once |
| setProperty() | Set CSS variables |
| getComputedStyle() | Read computed styles |

**Best Practice**: Use CSS classes for reusable styles; use style property for dynamic, JavaScript-calculated values.
