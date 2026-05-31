# Vue.js

## Definition

Vue.js is a **progressive JavaScript framework** for building user interfaces.

## Basic Example

```html
<script src="https://unpkg.com/vue@3"></script>

<div id="app">
    <h1>{{ message }}</h1>
    <button @click="reverseMessage">Reverse</button>
</div>

<script>
const app = Vue.createApp({
    data() {
        return {
            message: 'Hello Vue!'
        }
    },
    methods: {
        reverseMessage() {
            this.message = this.message.split('').reverse().join('');
        }
    }
});

app.mount('#app');
</script>
```

## Quick Revision

- Vue = progressive framework
- Reactive data binding
- Component-based
- Easy learning curve
- Use for: SPAs, UIs

---

## Related Topics

- [[What-is-Vue]] - [[What-is-Vue|Vue.js]]
- [[Vue.js]] - [[Vue.js|Vue.js]]
- [[What-is-Frameworks]] - [[What-is-Frameworks|Frameworks]]
- [[What-is-Components]] - [[What-is-Components|Components]]
