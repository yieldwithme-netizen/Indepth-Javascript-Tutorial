# Handle Form

## Definition

Form events fire when users interact with form elements. The main events are `submit`, `input`, `change`, and `focus`/`blur`. Use these to validate, process, and respond to form submissions.

## Code Examples

### Form Submission

```javascript
const form = document.getElementById('myForm');

form.addEventListener('submit', (event) => {
  event.preventDefault();
  
  const formData = new FormData(form);
  const name = formData.get('name');
  const email = formData.get('email');
  
  console.log('Name:', name);
  console.log('Email:', email);
});
```

### Input Validation

```javascript
const emailInput = document.getElementById('email');

emailInput.addEventListener('input', (event) => {
  const email = event.target.value;
  const isValid = /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
  
  emailInput.classList.toggle('valid', isValid);
  emailInput.classList.toggle('invalid', !isValid);
});
```

### Change Event

```javascript
const select = document.getElementById('mySelect');

select.addEventListener('change', (event) => {
  console.log('Selected value:', event.target.value);
});
```

### Focus and Blur

```javascript
const input = document.getElementById('myInput');

input.addEventListener('focus', () => {
  input.parentElement.classList.add('focused');
});

input.addEventListener('blur', () => {
  input.parentElement.classList.remove('focused');
});
```

### Form Data API

```javascript
form.addEventListener('submit', (event) => {
  event.preventDefault();
  
  const formData = new FormData(form);
  
  // Convert to object
  const data = Object.fromEntries(formData.entries());
  
  // Convert to JSON
  const jsonData = JSON.stringify(data);
  
  // Send with fetch
  fetch('/api/submit', {
    method: 'POST',
    body: formData
  });
});
```

### Form Validation

```javascript
const form = document.getElementById('myForm');

form.addEventListener('submit', (event) => {
  const inputs = form.querySelectorAll('[required]');
  let isValid = true;
  
  inputs.forEach(input => {
    if (!input.value.trim()) {
      isValid = false;
      input.classList.add('error');
    } else {
      input.classList.remove('error');
    }
  });
  
  if (!isValid) {
    event.preventDefault();
  }
});
```

## Common Form Events

| Event | When It Fires |
|-------|---------------|
| `submit` | Form submitted |
| `input` | Input value changes |
| `change` | Input loses focus after change |
| `focus` | Element gains focus |
| `blur` | Element loses focus |
| `reset` | Form reset button clicked |

## Common Use Cases

1. **Form validation** - Validate before submission
2. **AJAX submission** - Submit without page reload
3. **Real-time validation** - Validate as user types
4. **Auto-save** - Save form data periodically

## Common Mistakes

```javascript
// Wrong: Not preventing default submission
form.addEventListener('submit', (event) => {
  // Form will submit and page reloads
});

// Correct
form.addEventListener('submit', (event) => {
  event.preventDefault();
  // Handle submission
});

// Wrong: Using change instead of input for real-time
input.addEventListener('change', handler); // Only fires on blur

// Correct for real-time
input.addEventListener('input', handler); // Fires on every change
```

## Related Topics

- [[Prevent-Default]]
- [[Handle-Clicks]]
- [[Handle-Keys]]
- [[Use-Event-Props]]

## Quick Revision

| Event | Use Case |
|-------|----------|
| `submit` | Form submission |
| `input` | Real-time input validation |
| `change` | Select/checkbox changes |
| `focus/blur` | Input focus management |
| Always use | `preventDefault()` on submit |
