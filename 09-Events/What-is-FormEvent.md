# What is a Form Event?

## Definition

Form events fire when the user **interacts with form elements** (submit, change, input).

## Form Events

| Event | When It Fires |
|-------|---------------|
| submit | Form submitted |
| change | Input value changes (and loses focus) |
| input | Input value changes (real-time) |
| focus | Element gains focus |
| blur | Element loses focus |

## Examples

```javascript
// Form submit
form.addEventListener("submit", (e) => {
    e.preventDefault();
    const data = new FormData(form);
    console.log(Object.fromEntries(data));
});

// Input change
input.addEventListener("change", (e) => {
    console.log(`Changed to: ${e.target.value}`);
});

// Real-time input
input.addEventListener("input", (e) => {
    console.log(`Current value: ${e.target.value}`);
});

// Focus/blur
input.addEventListener("focus", () => {
    input.classList.add("focused");
});

input.addEventListener("blur", () => {
    input.classList.remove("focused");
});
```

## Getting Form Data

```javascript
// Method 1: FormData
const formData = new FormData(form);
const data = Object.fromEntries(formData);

// Method 2: Direct access
const name = form.elements.name.value;
const email = form.elements.email.value;

// Method 3: querySelector
const name = form.querySelector("#name").value;
```

## Quick Revision

- submit: form submitted (use preventDefault!)
- input: real-time value changes
- change: value changes on blur
- focus/blur: element focus
- Use FormData to get all values

---

## Related Topics

- [[Handle-Form]] - Form handling
- [[What-is-Event]] - Events
- [[What-is-EventObject]] - Event object
- [[Prevent-Default]] - preventDefault()
