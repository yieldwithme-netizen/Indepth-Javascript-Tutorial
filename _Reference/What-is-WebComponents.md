# Web Components

## Definition

Web Components are **reusable custom elements** for web pages.

## Basic Example

```javascript
class MyComponent extends HTMLElement {
    constructor() {
        super();
        this.attachShadow({ mode: 'open' });
    }
    
    connectedCallback() {
        this.shadowRoot.innerHTML = `<p>Hello!</p>`;
    }
}

customElements.define('my-component', MyComponent);
```

## Quick Revision

- Custom elements
- Shadow DOM for encapsulation
- HTML Templates
- Cross-framework compatible

---

## Related Topics

- [[What-is-WebComponents]] - [[What-is-WebComponents|Web components]]
- [[What-is-WebComponents]] - [[What-is-WebComponents|Web components]]
- [[Web-Components]] - [[Web-Components|Web components]]
- [[What-is-DOM]] - [[What-is-DOM|DOM]]
