# Docker Optimization Guide 🚀

## Image Optimization 📦

### Multi-stage Builds
```dockerfile
# Build stage
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# Production stage
FROM node:18-alpine
COPY --from=builder /app/dist ./dist
RUN npm ci --only=production
CMD ["node", "dist/main.js"]
```

### Layer Optimization
```dockerfile
# ✅ Combine commands
RUN apt-get update && \
    apt-get install -y \
      curl \
      nginx \
      postgresql-client && \
    rm -rf /var/lib/apt/lists/*

# ✅ Use .dockerignore
node_modules
npm-debug.log
Dockerfile
.git
.env
```

## Runtime Optimization ⚡

### Resource Limits
```yaml
version: '3.8'
services:
  app:
    deploy:
      resources:
        limits:
          cpus: '0.50'
          memory: 512M
        reservations:
          memory: 256M
```

### Container Settings
```bash
# Otimizar garbage collector
docker run -e "GOGC=100" app

# Tuning JVM
docker run \
  -e "JAVA_OPTS=-XX:+UseG1GC -XX:MaxRAMPercentage=75" \
  java-app
```

## Storage Optimization 💾

### Volume Performance
```bash
# tmpfs para dados temporários
docker run \
  --tmpfs /tmp:rw,noexec,nosuid,size=1g \
  nginx

# Volume otimizado
docker volume create --driver local \
  --opt type=ext4 \
  --opt device=/dev/sda1 \
  fast-storage
```

### I/O Tuning
```yaml
services:
  db:
    volumes:
      - data:/var/lib/postgresql/data
    blkio_config:
      weight: 1000
      device_read_bps:
        - path: /dev/sda
          rate: '100mb'
```

## Network Optimization 🌐

### DNS Settings
```bash
# DNS otimizado
docker run \
  --dns 8.8.8.8 \
  --dns-search example.com \
  nginx
```

### Network Mode
```bash
# Host networking para performance
docker run --network host nginx

# Container networking otimizado
docker run \
  --network-alias=fastnet \
  --dns=8.8.8.8 \
  nginx
```

## Build Optimization 🏗️

### BuildKit Features
```bash
# Habilitar BuildKit
export DOCKER_BUILDKIT=1

# Build paralelo
docker buildx build \
  --platform linux/amd64,linux/arm64 \
  --push .
```

### Cache Strategies
```dockerfile
# ✅ Otimizar cache
COPY package*.json ./
RUN npm ci
COPY . .

# Cache em registry
docker buildx build \
  --cache-from type=registry,ref=user/app:cache \
  --cache-to type=registry,ref=user/app:cache \
  .
```

## Memory Optimization 🧠

### Memory Management
```bash
# Limites de memória
docker run \
  --memory="512m" \
  --memory-swap="1g" \
  --memory-reservation="256m" \
  nginx

# OOM settings
docker run \
  --oom-kill-disable \
  --memory="1g" \
  nginx
```

## CPU Optimization 💻

### CPU Allocation
```bash
# CPU limits
docker run \
  --cpus=".5" \
  --cpu-shares=512 \
  nginx

# CPU pinning
docker run \
  --cpuset-cpus="0,1" \
  nginx
```

## Optimization Checklist ✅

### Build Time
1. [x] Multi-stage builds
2. [x] Layer optimization
3. [x] .dockerignore
4. [x] BuildKit enabled
5. [x] Cache strategies

### Runtime
1. [x] Resource limits
2. [x] Volume optimization
3. [x] Network tuning
4. [x] Memory management
5. [x] CPU allocation

## Monitoring & Metrics 📊

### Performance Metrics
```bash
# Container stats
docker stats --no-stream --format \
  "table {{.Container}}\t{{.CPUPerc}}\t{{.MemUsage}}"

# I/O metrics
docker stats --format \
  "table {{.Container}}\t{{.BlockIO}}\t{{.NetIO}}"
```

## Best Practices Matrix 📋

| Área | Otimização | Impacto |
|------|------------|---------|
| Build | Multi-stage | Redução de tamanho |
| Runtime | Resource limits | Estabilidade |
| Storage | Volume drivers | I/O Performance |
| Network | Host networking | Latência reduzida |

## Waifu Optimization Tips 💫

> **Build-chan diz:** "Camadas são como um bolo - quanto menos, melhor!"
{style="tip"}

> **Performance-sama alerta:** "Recursos são preciosos como cristais mágicos!"
{style="warning"}

## Quick Reference 📚

### Common Optimizations
```bash
# Build otimizado
DOCKER_BUILDKIT=1 docker build --no-cache .

# Runtime otimizado
docker run \
  --cpus=2 \
  --memory=2g \
  --network host \
  app

# Storage otimizado
docker volume create --driver local \
  --opt type=tmpfs \
  --opt device=tmpfs \
  --opt o=size=1g \
  fast-volume
```

> **Optimization Master's Note:** "A verdadeira otimização é um equilíbrio entre performance e recursos."
{style="note"}