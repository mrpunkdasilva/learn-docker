# Docker Caching Guide 🚀

## Build Cache Optimization 📦

### Layer Caching
```dockerfile
# ✅ Dependências primeiro
COPY package*.json ./
RUN npm ci

# ✅ Código fonte depois
COPY . .
RUN npm run build
```

### BuildKit Cache
```dockerfile
# ✅ Cache mount para build
RUN --mount=type=cache,target=/root/.cache/go-build \
    go build -o app

# ✅ Cache mount para package managers
RUN --mount=type=cache,target=/root/.npm \
    npm ci
```

## Registry Cache 🌐

### Registry Caching
```bash
# Cache em registry remoto
docker buildx build \
  --cache-from type=registry,ref=user/app:cache \
  --cache-to type=registry,ref=user/app:cache \
  .

# Pull com cache
docker pull --cache-from user/app:cache myapp
```

### Multi-registry Cache
```yaml
# docker-compose.yml
services:
  app:
    build:
      context: .
      cache_from:
        - registry-1.example.com/myapp:cache
        - registry-2.example.com/myapp:cache
```

## Local Cache Management 💾

### Volume Cache
```yaml
version: '3.8'
services:
  app:
    build: .
    volumes:
      # Cache de dependências
      - deps:/app/node_modules
      # Cache de build
      - build:/app/build

volumes:
  deps:
  build:
```

### Bind Mount Cache
```bash
# Cache de desenvolvimento
docker run \
  -v $(pwd):/app \
  -v node_modules:/app/node_modules \
  node:alpine npm install
```

## Cache Invalidation ⚡

### Estratégias de Invalidação
```dockerfile
# ✅ Cache bust com ARG
ARG CACHE_BUST=unknown
RUN echo "${CACHE_BUST}" && npm ci

# ✅ Cache com checksum
COPY package.json package-lock.json ./
RUN echo "$(md5sum package-lock.json)" > /tmp/hash \
    && npm ci
```

### Cache Cleanup
```bash
# Limpar cache local
docker builder prune --filter until=24h

# Limpar volumes de cache
docker volume rm $(docker volume ls -q -f name=*cache*)
```

## Development Cache 🔧

### Hot Reload Cache
```yaml
# docker-compose.dev.yml
services:
  app:
    volumes:
      - .:/app
      - node_modules:/app/node_modules
      - build_cache:/app/.next
    environment:
      - NODE_ENV=development

volumes:
  node_modules:
  build_cache:
```

### Multi-stage Cache
```dockerfile
# Build cache
FROM node:alpine AS deps
WORKDIR /app
COPY package*.json ./
RUN npm ci

# Development
FROM node:alpine AS dev
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules
COPY . .
CMD ["npm", "run", "dev"]

# Production build
FROM dev AS builder
RUN npm run build

# Production
FROM node:alpine
COPY --from=builder /app/dist ./dist
CMD ["npm", "start"]
```

## Cache Best Practices ✅

### DO's
1. [x] Use multi-stage builds
2. [x] Organize layers por mutabilidade
3. [x] Implemente BuildKit cache mounts
4. [x] Utilize registry caching
5. [x] Mantenha volumes de cache separados

### DON'Ts
1. [ ] Misturar dependências com código
2. [ ] Ignorar .dockerignore
3. [ ] Cachear dados sensíveis
4. [ ] Acumular cache desnecessário
5. [ ] Usar cache em produção sem necessidade

## Performance Matrix 📊

| Tipo de Cache | Uso | Performance |
|--------------|-----|-------------|
| Layer | Build | Alta |
| BuildKit | Build | Muito Alta |
| Registry | Distribuição | Média |
| Volume | Runtime | Alta |
| Bind Mount | Desenvolvimento | Média |

## Waifu Cache Tips 💫

> **Cache-chan diz:** "Cache é como mágica - use com sabedoria!"
{style="tip"}

> **Build-sama alerta:** "Muito cache pode ser tão ruim quanto pouco cache!"
{style="warning"}

## Quick Reference 📚

### Common Commands
```bash
# Rebuild sem cache
docker build --no-cache .

# Usar cache específico
docker build --cache-from myapp:cache .

# Limpar cache
docker builder prune

# Inspecionar cache
docker system df -v
```

### Cache Configuration
```yaml
# .dockerignore
node_modules
npm-debug.log
Dockerfile
.git
.cache

# docker-compose.yml
services:
  app:
    build:
      context: .
      cache_from:
        - myapp:cache
    volumes:
      - build-cache:/app/build

volumes:
  build-cache:
```

> **Cache Master's Note:** "O cache perfeito é aquele que você nem percebe que está lá."
{style="note"}