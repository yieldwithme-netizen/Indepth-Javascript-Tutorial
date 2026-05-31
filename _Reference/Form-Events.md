# Form Events

## Definition

Form events fire when users **interact with form elements**.

## Events

| Event | When |
|-------|------|
| submit | Form submitted |
| change | Value changes (on blur) |
| input | Value changes (real-time) |
| focus | Element gains focus |
| blur | Element loses focus |

## Examples

```javascript
// Submit
form.addEventListener("submit", (e) => {
    e.preventDefault();
    const data = new FormData(form);
    console.log(Object.fromEntries(data));
});

// Input
input.addEventListener("input", (e) => {
    console.log(`Current: ${e.target.value}`);
});
```

## Quick Revision

- submit: form submitted
- input: real-time changes
- change: value changes on blur
- focus/blur: element focus

---

## Related Topics

- [[What-is-FormEvent]] - [[What-is-FormEvent|Form events]]
- [[Handle-Form]] - [[Handle-Form|Form handling]]
- [[What-is-Event]] - [[What-is-Event|Events]]
- [[Prevent-Default]] - [[Prevent-Default|preventDefault]]
