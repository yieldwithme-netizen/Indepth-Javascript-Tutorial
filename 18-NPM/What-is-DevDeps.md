# What are dev dependencies

## Definition

Development dependencies (devDependencies) are packages that are only needed during the development process. They are not required for the application to run in production. Examples include testing frameworks, linting tools, build tools, and development servers.

## DevDependencies vs Dependencies

```json
{
  "dependencies": {
    "express": "^4.18.2",
    "lodash": "^4.17.21",
    "mongoose": "^7.6.0"
  },
  "devDependencies": {
    "nodemon": "^3.0.1",
    "jest": "^29.7.0",
    "eslint": "^8.50.0",
    "prettier": "^3.0.0"
  }
}
```

| Category | Purpose | Production |
|----------|---------|------------|
| dependencies | Runtime functionality | Installed |
| devDependencies | Development tools | Not installed |

## Common Dev Dependencies

### Testing Frameworks

```bash
npm install --save-dev jest
npm install --save-dev mocha
npm install --save-dev chai
npm install --save-dev @testing-library/react
```

### Code Quality Tools

```bash
npm install --save-dev eslint
npm install --save-dev prettier
npm install --save-dev husky
npm install --save-dev lint-staged
```

### Build Tools

```bash
npm install --save-dev webpack
npm install --save-dev babel
npm install --save-dev typescript
npm install --save-dev ts-node
```

### Development Servers

```bash
npm install --save-dev nodemon
npm install --save-dev live-server
npm install --save-dev http-server
```

### Type Definitions

```bash
npm install --save-dev @types/node
npm install --save-dev @types/express
npm install --save-dev @types/jest
```

## Installation Commands

```bash
# Install as devDependency
npm install package-name --save-dev
npm install package-name -D

# Install multiple devDependencies
npm install --save-dev jest eslint prettier
```

## package.json Structure

```json
{
  "name": "my-project",
  "version": "1.0.0",
  "scripts": {
    "test": "jest",
    "lint": "eslint src/",
    "dev": "nodemon index.js"
  },
  "dependencies": {
    "express": "^4.18.2"
  },
  "devDependencies": {
    "jest": "^29.7.0",
    "eslint": "^8.50.0",
    "nodemon": "^3.0.1"
  }
}
```

## Production Installation

When deploying to production, install only production dependencies:

```bash
# Install only dependencies (not devDependencies)
npm install --omit=dev
# or
npm install --production
```

## Common Use Cases

- Unit and integration testing
- Code linting and formatting
- Compilation and bundling
- Development server with hot reload
- Type checking and validation
- Code coverage reporting

## Common Mistakes

- Installing dev dependencies as regular dependencies
- Not specifying --save-dev flag
- Including dev dependencies in production builds
- Using outdated development tools
- Not configuring dev tools properly
- Mixing production and development configurations

## Related Topics

- [[What-is-PackageJSON]]
- [[Install-Packages]]
- [[What-is-Lock]]
- [[Create-Scripts]]
- [[NPM]]
- [[Testing]]
- [[Linting]]

## Quick Revision

| Term | Description |
|------|-------------|
| devDependencies | Development-only packages |
| --save-dev / -D | Flag to install as devDependency |
| Production install | `npm install --omit=dev` |
| Common types | Testing, linting, building, type definitions |
