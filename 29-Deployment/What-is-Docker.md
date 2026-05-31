# What is Docker?

## Definition

Docker is a **platform for building, deploying, and running applications in containers** — lightweight, isolated environments that package code with all dependencies.

## Containers vs Virtual Machines

| Feature | Containers | Virtual Machines |
|---------|------------|------------------|
| Size | Megabytes | Gigabytes |
| Startup | Seconds | Minutes |
| Isolation | Process-level | Full OS |
| Performance | Near-native | Slower |

## Basic Docker Commands

```bash
# Build image from Dockerfile
docker build -t my-app .

# Run container
docker run -p 3000:3000 my-app

# List running containers
docker ps

# Stop container
docker stop <container-id>

# Remove container
docker rm <container-id>
```

## Dockerfile for Node.js App

```dockerfile
# Dockerfile
FROM node:18-alpine

WORKDIR /app

COPY package*.json ./
RUN npm ci --only=production

COPY . .

EXPOSE 3000

CMD ["node", "server.js"]
```

## Docker Compose for Multi-Container Apps

```yaml
# docker-compose.yml
version: '3.8'

services:
  app:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
      - DATABASE_URL=postgresql://db:5432/mydb
    depends_on:
      - db

  db:
    image: postgres:15
    environment:
      - POSTGRES_DB=mydb
      - POSTGRES_PASSWORD=secret
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```

## .dockerignore

```bash
# .dockerignore
node_modules
npm-debug.log
.git
.env
.DS_Store
```

## Common Use Cases

```javascript
// 1. Consistent development environments
// "Works on my machine" problem solved

// 2. Microservices architecture
// Each service in its own container

// 3. CI/CD pipelines
// Build once, deploy anywhere

// 4. Scaling applications
// Run multiple container instances
```

## Common Mistakes

```dockerfile
# BAD: Large image size
FROM node:18
RUN apt-get update && apt-get install -y curl vim

# GOOD: Minimal image
FROM node:18-alpine

# BAD: Running as root
CMD ["node", "server.js"]

# GOOD: Create non-root user
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser
CMD ["node", "server.js"]
```

## Quick Revision

- Docker packages apps into containers
- Containers are lightweight and fast
- Dockerfile defines the image build process
- Docker Compose manages multi-container apps
- Use .dockerignore to exclude unnecessary files
- Always use specific image tags, not `latest`

---

## Related Topics

- [[What-is-CICD]] - CI/CD pipelines
- [[What-is-Deployment]] - Deployment overview
- [[What-is-Node]] - Node.js
- [[What-is-NPM]] - Package management
- [[What-is-Express]] - Express.js