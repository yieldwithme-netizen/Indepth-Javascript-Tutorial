# What is npx

## Definition

npx is a package executor tool that comes with npm (version 5.2.0+). It allows you to run npm packages without installing them globally or locally. npx can execute packages from the npm registry, local packages, and even arbitrary code.

## Basic Usage

### Run Package Without Installing

```bash
# Run create-react-app without installing
npx create-react-app my-app

# Run a one-time command
npx cowsay "Hello World"

# Run package from npm registry
npx http-server
```

### Run Local Package

```bash
# Run locally installed package
npx jest

# Run with specific options
npx jest --coverage

# Run binary from local node_modules
npx eslint src/
```

## Common Use Cases

### Create Projects

```bash
# Create React app
npx create-react-app my-app

# Create Next.js app
npx create-next-app my-app

# Create Express app
npx express-generator my-app

# Create Vue app
npx @vue/cli create my-app
```

### Code Generation

```bash
# Generate component
npx generate-component MyComponent

# Create boilerplate
npx degit user/repo my-project
```

### Testing and Linting

```bash
# Run tests without installing jest
npx jest

# Lint code without installing eslint
npx eslint src/

# Format code
npx prettier --write src/
```

### Development Servers

```bash
# Start a static server
npx http-server

# Start with port
npx http-server -p 8080

# Live reload server
npx live-server
```

## npx vs npm

```bash
# npm: Install then run
npm install -g create-react-app
create-react-app my-app

# npx: Run directly
npx create-react-app my-app
```

| Feature | npm | npx |
|---------|-----|-----|
| Installation | Required | Optional |
| Global install | Yes | No |
| Use case | Repeated use | One-time use |
| Cache | node_modules | npm cache |

## Running Specific Versions

```bash
# Run specific version
npx create-react-app@5.0.0 my-app

# Run with version range
npx jest@^29.0.0

# Run latest version explicitly
npx package-name@latest
```

## Passing Arguments

```bash
# Pass arguments to package
npx create-react-app my-app --template typescript

# Run with flags
npx jest --coverage --verbose

# Multiple arguments
npx eslint src/ --fix --ext .js,.jsx
```

## Caching Behavior

```bash
# npx caches downloaded packages
# First run downloads to cache
# Subsequent runs use cache

# Clear npx cache
npm cache clean --force
```

## Custom Packages from GitHub

```bash
# Run package from GitHub
npx github:user/repo

# Run specific branch
npx github:user/repo#branch-name
```

## Common Use Cases

- Running create- packages (React, Next, Vue)
- One-off command execution
- Avoiding global installations
- Testing packages before installing
- Running development tools
- Prototyping and experimentation

## Common Mistakes

- Using npx for packages you use frequently (install locally instead)
- Not specifying version when reproducibility matters
- Using npx in scripts (prefer local dependencies)
- Ignoring security warnings from npx
- Running untrusted packages without review

## Related Topics

- [[What-is-PackageJSON]]
- [[Install-Packages]]
- [[What-is-DevDeps]]
- [[Create-Scripts]]
- [[NPM]]
- [[Node-Modules]]

## Quick Revision

| Command | Purpose |
|---------|---------|
| `npx package` | Run package without install |
| `npx package@version` | Run specific version |
| `npx create-*` | Create project templates |
| `npx eslint` | Run linting without install |
| `npx jest` | Run tests without install |
