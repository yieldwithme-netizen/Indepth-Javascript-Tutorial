# How to Initialize npm Project

## Definition

Initializing an npm project creates a `package.json` file that serves as the configuration file for your Node.js project. This process sets up the project metadata, dependencies tracking, and scripts.

## Using npm init

### Interactive Mode

```bash
npm init
```

This starts an interactive wizard that prompts for project details:

```
package name: (current-directory)
version: (1.0.0)
description: My awesome project
entry point: (index.js)
test command: jest
git repository: https://github.com/user/repo.git
keywords: javascript, node
author: John Doe
license: (ISC)
```

Press Enter to accept defaults or provide custom values.

### Using Yes Flag (Quick Init)

```bash
npm init -y
# or
npm init --yes
```

Creates a `package.json` with all default values without prompts.

## Using Package Name

```bash
npm init my-app
```

Uses the `create-my-app` package from npm registry.

## Custom Init with Scope

```bash
npm init --scope=@myorg
```

Creates a scoped package under your organization.

## Generated package.json

```json
{
  "name": "my-project",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

## Alternative: Using yarn

```bash
yarn init
yarn init -y
```

## Alternative: Using pnpm

```bash
pnpm init
```

## Post-Initialization Steps

### 1. Install Dependencies

```bash
npm install express lodash axios
```

### 2. Create Project Structure

```bash
mkdir -p src tests
touch src/index.js
```

### 3. Setup .gitignore

```bash
echo "node_modules/" > .gitignore
echo ".env" >> .gitignore
```

### 4. Initialize Git Repository

```bash
git init
git add .
git commit -m "Initial commit"
```

## Common Use Cases

- Starting new Node.js applications
- Creating reusable npm packages
- Setting up microservices
- Initializing frontend projects
- Creating CLI tools

## Common Mistakes

- Not initializing git after npm init
- Forgetting to create .gitignore
- Not setting up proper scripts
- Skipping description and keywords for packages
- Not specifying the correct main entry point

## Related Topics

- [[What-is-PackageJSON]]
- [[Install-Packages]]
- [[Create-Scripts]]
- [[NPM]]
- [[Node-Modules]]
- [[Version-Control]]

## Quick Revision

| Command | Description |
|---------|-------------|
| `npm init` | Interactive initialization |
| `npm init -y` | Quick init with defaults |
| `npm init --scope=@org` | Scoped package init |
| `yarn init` | Yarn equivalent |
| `pnpm init` | pnpm equivalent |
