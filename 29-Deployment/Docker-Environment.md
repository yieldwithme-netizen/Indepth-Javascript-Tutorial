# Docker Environment

## Definition

Docker environment **containers** for consistent development.

## Basic Dockerfile

```dockerfile
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["node", "index.js"]
```

## Quick Revision

- Docker = containerization
- Dockerfile defines environment
- docker-compose for multi-container
- Consistent across machines

---

## Related Topics

- [[What-is-Docker]] - [[What-is-Docker|Docker]]
- [[Docker-Environment]] - [[Docker-Environment|Docker environment]]
- [[What-is-Docker]] - [[What-is-Docker|Docker]]
- [[Deploy-Netlify]] - [[Deploy-Netlify|Deployment]]
