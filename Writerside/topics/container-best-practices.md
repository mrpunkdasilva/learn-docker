# Container Best Practices 🏆

## Core Principles Matrix 📊

```ascii
┌────────────────────────────┐
│ CONTAINER BEST PRACTICES   │
│                           │
│ 1. Security First         │
│ 2. Resource Efficiency    │
│ 3. Maintainability       │
│ 4. Scalability           │
│ 5. Monitoring            │
└────────────────────────────┘
```

## Image Best Practices 🖼️

### DO's ✅
```dockerfile
# Use versões específicas
FROM node:18.12.1-alpine

# Multi-stage builds
FROM node:18.12.1-alpine AS builder
WORKDIR /app
COPY . .
RUN npm ci && npm run build

FROM node:18.12.1-alpine
COPY --from=builder /app/dist ./dist
CMD ["node", "dist/main.js"]

# Agrupar comandos RUN
RUN apt-get update && \
    apt-get install -y \
    package1 \
    package2 && \
    rm -rf /var/lib/apt/lists/*
```

### DON'Ts ❌
```dockerfile
# Evite latest tag
FROM node:latest

# Evite múltiplos RUN
RUN apt-get update
RUN apt-get install package1
RUN apt-get install package2

# Não armazene secrets
ENV API_KEY="secret123"
```

## Security Guidelines 🔒

### Container Hardening
```dockerfile
# Use non-root user
FROM alpine
RUN adduser -D appuser
USER appuser

# Read-only root filesystem
docker run --read-only nginx
```

### Secrets Management
```bash
# Use Docker secrets
docker secret create app_secret secret.txt
docker service create \
    --secret app_secret \
    myapp

# Evite environment variables para secrets
docker run -e "API_KEY=secret" myapp
```

## Resource Management 📊

### Resource Limits
```bash
# Set resource limits
docker run \
    --cpus=".5" \
    --memory="512m" \
    --memory-swap="1g" \
    nginx
```

### Monitoring Setup
```bash
# Health checks
HEALTHCHECK --interval=5m --timeout=3s \
  CMD curl -f http://localhost/ || exit 1

# Resource monitoring
docker stats --format "table {{.Container}}\t{{.CPUPerc}}\t{{.MemUsage}}"
```

## Networking Best Practices 🌐

### Network Security
```bash
# Use redes customizadas
docker network create --driver bridge mynetwork

# Isole containers
docker run --network mynetwork nginx
```

### Network Configuration
```yaml
version: '3.8'
services:
  web:
    networks:
      frontend:
      backend:
networks:
  frontend:
    driver: bridge
  backend:
    driver: bridge
    internal: true
```

## Storage Best Practices 💾

### Volume Management
```bash
# Named volumes
docker volume create mydata
docker run -v mydata:/data nginx

# Backup volumes
docker run --rm -v mydata:/data \
    -v $(pwd):/backup alpine \
    tar cvf /backup/backup.tar /data
```

## Logging Best Practices 📝

### Log Configuration
```bash
# JSON log driver
docker run --log-driver json-file \
    --log-opt max-size=10m \
    --log-opt max-file=3 \
    nginx
```

### Log Aggregation
```yaml
logging:
  driver: "fluentd"
  options:
    fluentd-address: localhost:24224
    tag: docker.{{.Name}}
```

## Development Workflow 🔄

### Development Environment
```yaml
version: '3.8'
services:
  dev:
    build:
      context: .
      target: development
    volumes:
      - .:/app
      - /app/node_modules
    command: npm run dev
```

### Testing Environment
```yaml
version: '3.8'
services:
  test:
    build:
      context: .
      target: test
    command: npm test
```

## Production Checklist ✅

### Pre-Deployment
1. [ ] Imagem otimizada
2. [ ] Security scan realizado
3. [ ] Resources limits definidos
4. [ ] Health checks implementados
5. [ ] Logs configurados
6. [ ] Backups planejados
7. [ ] Monitoring setup

### Post-Deployment
1. [ ] Performance verificada
2. [ ] Logs analisados
3. [ ] Métricas coletadas
4. [ ] Alertas configurados
5. [ ] Backup testado

## Best Practices Matrix 📋

### Security
| Prática | Descrição | Prioridade |
|---------|-----------|------------|
| Non-root user | Execute como usuário não-root | Alta |
| Read-only FS | Use filesystem read-only | Alta |
| Scan images | Scanner de vulnerabilidades | Alta |
| Secrets | Use Docker secrets | Alta |

### Performance
| Prática | Descrição | Prioridade |
|---------|-----------|------------|
| Resource limits | Defina limites de recursos | Alta |
| Multi-stage | Use multi-stage builds | Média |
| Layer caching | Otimize cache de layers | Média |
| Compression | Comprima artefatos | Baixa |

## Waifu Best Practice Tips 💡

> **Security-chan diz:** "Nunca confie em imagens não verificadas! 🔍"
{style="tip"}

> **Performance-sama avisa:** "Containers leves são containers felizes! 🎈"
{style="warning"}

## Quick Reference 📚

### Essential Commands
```bash
# Security scan
docker scan myimage

# Resource monitoring
docker stats

# Health check
docker inspect --format='{{.State.Health.Status}}' container_name

# Network inspect
docker network inspect bridge
```

## Next Steps 🎯

1. [Security Hardening](container-security.md)
2. [Performance Optimization](docker-performance.md)
3. [Monitoring Setup](container-monitoring.md)

> **Best Practice Master's Note:** "Boas práticas são como um kata - devem ser praticadas até se tornarem naturais."
{style="note"}