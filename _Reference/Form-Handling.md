# Form Handling

## Definition

Form handling in JavaScript involves capturing, validating, processing, and submitting user input from HTML forms. Modern JavaScript provides multiple approaches to form handling, from traditional DOM events to React-style controlled components. Proper form handling ensures data integrity and a smooth user experience.

## Code Examples

### Basic Form Event Handling

```html
<form id="myForm">
  <input type="text" name="username" required />
  <input type="email" name="email" required />
  <button type="submit">Submit</button>
</form>
```

```javascript
const form = document.getElementById("myForm");

form.addEventListener("submit", function (event) {
  event.preventDefault(); // Prevent page reload

  const formData = new FormData(form);
  const username = formData.get("username");
  const email = formData.get("email");

  console.log({ username, email });
});
```

### FormData API

```javascript
const form = document.getElementById("myForm");

form.addEventListener("submit", function (event) {
  event.preventDefault();

  // Create FormData from form
  const formData = new FormData(form);

  // Get individual values
  const name = formData.get("name");
  const email = formData.get("email");

  // Convert to object
  const data = Object.fromEntries(formData.entries());
  console.log(data);

  // Iterate over all fields
  for (const [key, value] of formData) {
    console.log(`${key}: ${value}`);
  }
});
```

### Input Validation

```javascript
class FormValidator {
  constructor(form) {
    this.form = form;
    this.errors = {};
  }

  addRule(fieldName, rule) {
    if (!this.rules) this.rules = {};
    if (!this.rules[fieldName]) this.rules[fieldName] = [];
    this.rules[fieldName].push(rule);
    return this;
  }

  validate() {
    this.errors = {};

    for (const [field, rules] of Object.entries(this.rules || {})) {
      const value = this.form[field]?.value;

      for (const rule of rules) {
        const error = rule(value, field);
        if (error) {
          this.errors[field] = error;
          break;
        }
      }
    }

    return Object.keys(this.errors).length === 0;
  }

  getErrors() {
    return this.errors;
  }
}

// Usage
const form = document.getElementById("signupForm");
const validator = new FormValidator(form);

validator
  .addRule("email", (value) => {
    if (!value) return "Email is required";
    if (!/\S+@\S+\.\S+/.test(value)) return "Invalid email";
    return null;
  })
  .addRule("password", (value) => {
    if (!value) return "Password is required";
    if (value.length < 8) return "Password must be at least 8 characters";
    return null;
  });

form.addEventListener("submit", function (event) {
  event.preventDefault();

  if (validator.validate()) {
    // Submit the form
    submitForm(new FormData(form));
  } else {
    const errors = validator.getErrors();
    Object.entries(errors).forEach(([field, message]) => {
      showFieldError(field, message);
    });
  }
});
```

### Real-time Validation

```javascript
const inputs = document.querySelectorAll("input[data-validate]");

inputs.forEach((input) => {
  input.addEventListener("input", function () {
    const error = validateField(this);
    const errorEl = this.parentElement.querySelector(".error");

    if (error) {
      this.classList.add("invalid");
      this.classList.remove("valid");
      if (errorEl) errorEl.textContent = error;
    } else {
      this.classList.remove("invalid");
      this.classList.add("valid");
      if (errorEl) errorEl.textContent = "";
    }
  });
});

function validateField(input) {
  const rules = JSON.parse(input.dataset.validate || "{}");

  if (rules.required && !input.value.trim()) {
    return "This field is required";
  }
  if (rules.minLength && input.value.length < rules.minLength) {
    return `Minimum ${rules.minLength} characters`;
  }
  if (rules.pattern && !new RegExp(rules.pattern).test(input.value)) {
    return "Invalid format";
  }
  return null;
}
```

### Form Submission with Fetch

```javascript
async function submitForm(formData) {
  try {
    const response = await fetch("/api/submit", {
      method: "POST",
      body: formData,
    });

    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }

    const result = await response.json();
    showSuccess("Form submitted successfully!");
    return result;
  } catch (error) {
    showError("Submission failed: " + error.message);
  }
}

// JSON submission
async function submitJSON(data) {
  try {
    const response = await fetch("/api/submit", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify(data),
    });

    return await response.json();
  } catch (error) {
    console.error("Error:", error);
  }
}
```

### File Upload

```javascript
const fileInput = document.querySelector('input[type="file"]');
const form = document.getElementById("uploadForm");

form.addEventListener("submit", async function (event) {
  event.preventDefault();

  const formData = new FormData();
  const files = fileInput.files;

  for (let i = 0; i < files.length; i++) {
    formData.append("files", files[i]);
  }

  try {
    const response = await fetch("/api/upload", {
      method: "POST",
      body: formData, // Don't set Content-Type header
    });

    const result = await response.json();
    console.log("Uploaded:", result);
  } catch (error) {
    console.error("Upload failed:", error);
  }
});
```

### Dynamic Form Fields

```javascript
class DynamicForm {
  constructor(container) {
    this.container = container;
    this.fieldCount = 0;
  }

  addField(type = "text", name = null) {
    this.fieldCount++;
    const fieldName = name || `field_${this.fieldCount}`;

    const wrapper = document.createElement("div");
    wrapper.className = "form-field";
    wrapper.innerHTML = `
      <label for="${fieldName}">${fieldName}</label>
      <input type="${type}" name="${fieldName}" id="${fieldName}" />
      <button type="button" class="remove-field">Remove</button>
    `;

    wrapper.querySelector(".remove-field").addEventListener("click", () => {
      wrapper.remove();
    });

    this.container.appendChild(wrapper);
  }

  getFields() {
    return Array.from(this.container.querySelectorAll("input")).map(
      (input) => ({
        name: input.name,
        type: input.type,
        value: input.value,
      })
    );
  }
}

// Usage
const dynamicForm = new DynamicForm(document.getElementById("fields"));
dynamicForm.addField("text", "name");
dynamicForm.addField("email", "email");
```

## Common Use Cases

- **User registration** — Capture and validate signup data
- **Login forms** — Authenticate users
- **Search forms** — Process search queries
- **Surveys** — Collect structured responses
- **File uploads** — Handle file submissions
- **Multi-step forms** — Wizard-style form flows

## Common Mistakes

```javascript
// Mistake 1: Not preventing default form submission
form.addEventListener("submit", function (event) {
  // Missing event.preventDefault()
  // Page will reload!
});

// Mistake 2: Not validating on server
// Client-side validation is for UX only
// Always validate on the server too

// Mistake 3: Using innerHTML with user input (XSS)
const userInput = '<script>alert("xss")</script>';
// Bad: container.innerHTML = userInput;
// Good: container.textContent = userInput;

// Mistake 4: Not handling disabled fields
const formData = new FormData(form);
// Disabled fields are NOT included in FormData
// If you need them, enable before collecting

// Mistake 5: Forgetting to handle form reset
form.addEventListener("reset", function () {
  // Clear any error messages
  document.querySelectorAll(".error").forEach((el) => {
    el.textContent = "";
  });
});
```

## Related Topics

- [[Event-Handling]]
- [[Add-Event-Listeners]]
- [[DOM-Manipulation]]
- [[Fetch-API]]
- [[Async-Await]]
- [[String-Methods]]
- [[Validation]]

## Quick Revision

| API | Purpose |
|-----|---------|
| `FormData` | Collect form data easily |
| `event.preventDefault()` | Stop default form submission |
| `form.submit()` | Submit form programmatically |
| `form.reset()` | Reset form to initial state |
| `form.checkValidity()` | Check if form is valid |
| `fetch()` | Submit form data via HTTP |

| Best Practice | Description |
|---------------|-------------|
| Always validate server-side | Client validation is UX only |
| Prevent default | Handle submission with JS |
| Use FormData | Clean way to collect data |
| Show errors | Display validation feedback |
| Handle disabled | They're excluded from FormData |
| Sanitize input | Prevent XSS attacks |
