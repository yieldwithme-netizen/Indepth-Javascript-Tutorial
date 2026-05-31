# Web Components

## Definition

Web Components are **reusable, custom elements** for web pages.

## Basic Example

```javascript
class MyComponent extends HTMLElement {
    constructor() {
        super();
        this.attachShadow({ mode: 'open' });
    }
    
    connectedCallback() {
        this.shadowRoot.innerHTML = `
            <style>
                p { color: red; }
            </style>
            <p>Hello, World!</p>
        `;
    }
}

customElements.define('my-component', MyComponent);
```

## Usage

```html
<my-component></my-component>
```

## Quick Revision

- Web Components = custom elements
- Shadow DOM for encapsulation
- Custom Elements API
- HTML Templates
- Cross-framework compatibility

---

## Related Topics

- [[What-is-WebComponents]] - [[What-is-WebComponents|Web components]]
- [[Web-Components]] - [[Web-Components|Web components]]
- [[What-is-DOM]] - [[What-is-DOM|DOM]]
- [[What-is-Class]] - [[What-is-Class|Classes]]
