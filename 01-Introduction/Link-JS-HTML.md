# How to Link JavaScript to HTML

## Method 1: Internal Script (Inline)

```html
<!DOCTYPE html>
<html>
<head>
    <title>Internal JS</title>
</head>
<body>
    <h1>Hello</h1>
    
    <script>
        // JavaScript goes here
        console.log("Internal script");
        document.querySelector("h1").style.color = "blue";
    </script>
</body>
</html>
```

## Method 2: External Script (Recommended)

### Step 1: Create JS file

`script.js`:
```javascript
console.log("External script loaded!");
document.querySelector("h1").style.color = "blue";
```

### Step 2: Link in HTML

```html
<!DOCTYPE html>
<html>
<head>
    <title>External JS</title>
</head>
<body>
    <h1>Hello</h1>
    <script src="script.js"></script>
</body>
</html>
```

## Method 3: Module Script

```html
<script type="module" src="app.js"></script>
```

## Script Tag Attributes

| Attribute | Purpose | Example |
|-----------|---------|---------|
| `src` | External file path | `src="app.js"` |
| `type` | MIME type | `type="module"` |
| `async` | Load without blocking | `async` |
| `defer` | Load after HTML parsed | `defer` |
| `charset` | Character encoding | `charset="UTF-8"` |

## [[async]] vs [[defer]]

```html
<!-- Blocks HTML parsing until script loads -->
<script src="app.js"></script>

<!-- Loads in parallel, executes immediately -->
<script src="app.js" async></script>

<!-- Loads in parallel, executes after HTML parsed -->
<script src="app.js" defer></script>
```

### When to Use Each

| Scenario | Use |
|----------|-----|
| Simple page, no dependencies | `async` |
| Script depends on [[DOM]] | `defer` |
| Module imports | `type="module"` |
| Quick test | Internal script |

## Script Placement

```html
<body>
    <!-- ❌ Bad: May run before elements exist -->
    <script src="app.js"></script>
    
    <h1>Hello</h1>
    <p>Content</p>
    
    <!-- ✅ Good: Runs after elements are loaded -->
    <script src="app.js"></script>
</body>

<!-- ✅ Best: Use defer -->
<head>
    <script src="app.js" defer></script>
</head>
```

## Common Errors

```javascript
// ❌ Wrong: File not found
<script src="scipt.js"></script>  // typo!

// ❌ Wrong: Path incorrect
<script src="js/app.js"></script>  // if file is in root

// ✅ Right: Correct path
<script src="js/app.js"></script>  // if file is in js folder
```

## Quick Revision

1. Use `<script src="file.js"></script>` for external files
2. Place scripts at end of `<body>` or use `defer`
3. `async` = load and run immediately
4. `defer` = load now, run after HTML parsed
5. Always check file paths

---

## Related Topics

- [[First-JS-File]] - Creating JS files
- [[What-is-DOM]] - Understanding DOM
- [[What-is-Module]] - JavaScript modules
- [[What-is-ES6]] - ES6 features