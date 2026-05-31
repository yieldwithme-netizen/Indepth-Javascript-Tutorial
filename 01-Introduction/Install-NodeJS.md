# How to Install Node.js

## What is Node.js?

Node.js is a **JavaScript runtime** that lets you run [[JavaScript]] **outside the browser** (on your computer/server).

## Step 1: Download Node.js

1. Go to https://nodejs.org
2. Choose **LTS version** (Long Term Support - recommended)
3. Download for your OS

## Step 2: Install

### Windows
- Run the `.msi` installer
- Click "Next" through all steps
- Accept license agreement

### Mac
- Run the `.pkg` installer
- Follow prompts

### Linux
```bash
# Using apt (Ubuntu/Debian)
sudo apt update
sudo apt install nodejs npm

# Using nvm (recommended)
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
nvm install --lts
```

## Step 3: Verify Installation

Open terminal/command prompt:

```bash
# Check Node version
node --version
# Output: v18.17.0 (or similar)

# Check npm version
npm --version
# Output: 9.6.7 (or similar)
```

## Step 4: Your First Node.js Program

Create `hello.js`:

```javascript
console.log("Hello from Node.js!");
```

Run it:

```bash
node hello.js
# Output: Hello from Node.js!
```

## Node.js vs Browser [[JavaScript]]

| Feature | Browser | Node.js |
|---------|---------|---------|
| [[window]] object | ✅ | ❌ |
| [[document]] object | ✅ | ❌ |
| fs (file system) | ❌ | ✅ |
| process object | ❌ | ✅ |
| npm packages | ❌ | ✅ |

## Using npm (Node Package Manager)

```bash
# Initialize a new project
npm init -y

# Install a package
npm install lodash

# Install dev dependency
npm install --save-dev nodemon

# Install globally
npm install -g typescript

# See installed packages
npm list
```

## Running Node.js

```bash
# Run a file
node app.js

# Run with watch mode (auto-restart)
node --watch app.js

# Using nodemon (auto-restart)
npx nodemon app.js

# Enter interactive mode (REPL)
node
```

## Quick Revision

1. Download from nodejs.org (LTS version)
2. Install with default settings
3. Verify: `node --version` and `npm --version`
4. Run JS files: `node filename.js`
5. npm = package manager for installing libraries

---

## Related Topics

- [[What-is-JS-Runtime]] - Understanding JS runtime
- [[What-is-NPM]] - Package management
- [[First-JS-File]] - Creating JS files
- [[What-is-Node]] - Node.js fundamentals