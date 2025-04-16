# Melhores Práticas Docker: O Caminho do Mestre 🎯

```ascii
╔═══════════════════════════════════════════════════════════╗
║ DOCKER BEST PRACTICES MATRIX                              ║
║                                                          ║
║    Security ◄────► Performance ◄────► Maintainability    ║
║        ▲              ▲                ▲                 ║
║        │              │                │                 ║
║        └──────────────┴────────────────┘                ║
║                   DevOps Flow                           ║
║                                                          ║
╚═══════════════════════════════════════════════════════════╝
```

## Dockerfile Best Practices 📝

### Otimização de Imagens
```dockerfile
# ✅ Multi-stage build
FROM node:alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

FROM nginx:alpine
COPY --from=builder /app/dist /usr/share/nginx/html
```

### Layer Optimization
```dockerfile
# ✅ Combine RUN commands
RUN apt-get update && \
    apt-get install -y \
    package1 \
    package2 && \
    rm -rf /var/lib/apt/lists/*

# ❌ Avoid multiple RUN commands
RUN apt-get update
RUN apt-get install package1
RUN apt-get install package2
```

## Security Guidelines 🔒

### Container Hardening
```dockerfile
# ✅ Use non-root user
FROM alpine
RUN adduser -D appuser
USER appuser

# ✅ Read-only root filesystem
docker run --read-only nginx
```

### Secrets Management
```bash
# ✅ Use Docker secrets
docker secret create app_secret secret.txt
docker service create \
    --secret app_secret \
    myapp

# ❌ Avoid environment variables for secrets
docker run -e "API_KEY=secret" myapp
```

## Performance Optimization 🚀

### Resource Management
```bash
# ✅ Set resource limits
docker run \
    --cpus=".5" \
    --memory="512m" \
    --memory-swap="1g" \
    nginx
```

### Networking
```bash
# ✅ Use user-defined networks
docker network create --driver overlay mynet

# ✅ Enable DNS caching
docker run --dns-opt="ndots:1" nginx
```

## Development Workflow 💻

### Docker Compose
```yaml
# ✅ docker-compose.yml
version: '3.8'
services:
  app:
    build:
      context: .
      target: development
    volumes:
      - .:/app
      - /app/node_modules
    environment:
      - NODE_ENV=development
```

### Testing
```bash
# ✅ Dedicated test container
docker-compose -f docker-compose.test.yml up
```

## Production Deployment 🌟

### Health Checks
```dockerfile
# ✅ Add HEALTHCHECK
HEALTHCHECK --interval=30s --timeout=3s \
  CMD curl -f http://localhost/ || exit 1
```

### Logging
```bash
# ✅ Configure logging
docker run \
    --log-driver json-file \
    --log-opt max-size=10m \
    --log-opt max-file=3 \
    nginx
```

## CI/CD Integration 🔄

### Build Pipeline
```yaml
# ✅ GitHub Actions example
name: Docker Build

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Build and test
        run: |
          docker build -t myapp:test .
          docker run myapp:test npm test
```

## Monitoring & Maintenance 📊

### Container Health
```bash
# ✅ Regular health checks
docker inspect --format='{{.State.Health.Status}}' container_name

# ✅ Resource monitoring
docker stats --format "table {{.Container}}\t{{.CPUPerc}}\t{{.MemUsage}}"
```

## DO's and DON'Ts Matrix ✨

### DO's ✅
```ascii
┌────────────────────────────────┐
│ 1. Use multi-stage builds      │
│ 2. Set resource limits         │
│ 3. Implement health checks     │
│ 4. Use .dockerignore          │
│ 5. Version control images      │
└────────────────────────────────┘
```

### DON'Ts ❌
```ascii
┌────────────────────────────────┐
│ 1. Run as root                 │
│ 2. Store secrets in images     │
│ 3. Use latest tag             │
│ 4. Ignore security scans      │
│ 5. Skip health checks         │
└────────────────────────────────┘
```

## Waifu Best Practice Tips 💡

> **Security-chan diz:** "Nunca confie em imagens não verificadas! 🔍"
{style="tip"}

> **Performance-sama avisa:** "Containers leves são containers felizes! 🎈"
{style="warning"}

## Checklist de Qualidade ✅

### Antes do Deploy
1. [ ] Imagem otimizada
2. [ ] Security scan realizado
3. [ ] Resources limits definidos
4. [ ] Health checks implementados
5. [ ] Logs configurados
6. [ ] Backups planejados
7. [ ] Monitoring setup

## Troubleshooting Guide 🔧

### Common Issues
```bash
# ✅ Debug container
docker logs container_name

# ✅ Interactive debug
docker exec -it container_name sh

# ✅ Network debug
docker network inspect bridge
```

## Próximos Passos 🎯

1. [Laboratório Prático](hands-on-lab.md)
2. [Guia de Troubleshooting](troubleshooting-guide.md)
3. [Certificação Docker](docker-certified-topics.md)

> **Sensei's Note:** "As melhores práticas são como um kata - devem ser praticadas até se tornarem naturais."
{style="note"}