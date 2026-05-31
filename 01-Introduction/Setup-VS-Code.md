# How to Set Up VS Code for JavaScript

## Step 1: Install VS Code

1. Go to https://code.visualstudio.com
2. Download for your OS (Windows/Mac/Linux)
3. Run installer, accept defaults
4. Launch VS Code

## Step 2: Install Essential Extensions

Click Extensions icon (left sidebar) or press `Ctrl+Shift+X`:

| Extension | Purpose | Install Command |
|-----------|---------|-----------------|
| **ESLint** | Code linting | `ext install dbaeumer.vscode-eslint` |
| **Prettier** | Code formatting | `ext install esbenp.prettier-vscode` |
| **Live Server** | Auto-reload browser | `ext install ritwickdey.liveserver` |
| **JavaScript (ES6)** | Snippets | `ext install xabikos.javascriptsnippets` |
| **Auto Rename Tag** | HTML tag sync | `ext install formulahendry.auto-rename-tag` |

## Step 3: Configure Settings

Press `Ctrl+Shift+P` → "Open User Settings (JSON)":

```json
{
    "editor.formatOnSave": true,
    "editor.defaultFormatter": "esbenp.prettier-vscode",
    "editor.tabSize": 2,
    "editor.wordWrap": "on",
    "javascript.updateImportsOnFileMove.enabled": "always",
    "suggest.showKeywords": true,
    "emmet.includeLanguages": {
        "javascript": "javascriptreact"
    }
}
```

## Step 4: Create Project Structure

```
my-project/
├── index.html
├── css/
│   └── style.css
├── js/
│   └── main.js
└── assets/
```

## Step 5: Configure Launch (Debugging)

Create `.vscode/launch.json`:

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "type": "chrome",
            "request": "launch",
            "name": "Launch Chrome",
            "url": "http://localhost:5500",
            "webRoot": "${workspaceFolder}"
        }
    ]
}
```

## Essential Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| `Ctrl+Space` | Trigger suggestions |
| `Ctrl+Shift+P` | Command palette |
| `Ctrl+P` | Quick file open |
| `F12` | Go to definition |
| `Alt+Up/Down` | Move line |
| `Shift+Alt+Up/Down` | Copy line |
| `Ctrl+/` | Toggle comment |
| `Ctrl+D` | Select next occurrence |

## Quick Revision

1. Install VS Code from code.visualstudio.com
2. Install ESLint + Prettier extensions
3. Enable format on save
4. Create proper project structure
5. Set up [[debugging]] configuration

---

## Related Topics

- [[What-is-JavaScript]] - JavaScript basics
- [[First-JS-File]] - Creating your first JS file
- [[Use-Chrome-DevTools]] - Browser [[debugging]]
- [[What-is-ESLint]] - Code linting
- [[What-is-Prettier]] - Code formatting