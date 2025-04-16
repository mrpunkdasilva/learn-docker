# Dockerfile Examples: Exemplos do Mundo Real 🌍

```ascii
╔════════════════════════════════════════════════════════════╗
║ DOCKERFILE EXAMPLES COLLECTION                             ║
║                                                           ║
║ Web Apps │ APIs │ Databases │ CLI Tools │ Microservices   ║
║                                                           ║
║ Development │ Production │ Multi-stage │ Optimized        ║
╚════════════════════════════════════════════════════════════╝
```

## Web Applications 🌐

### Node.js Application
```dockerfile
# Development
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm", "run", "dev"]

# Production
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

FROM node:18-alpine
WORKDIR /app
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/package*.json ./
RUN npm ci --only=production
EXPOSE 3000
CMD ["npm", "start"]
```

### React + Nginx
```dockerfile
# Build stage
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# Production stage
FROM nginx:alpine
COPY --from=builder /app/build /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

## Backend Services 🔧

### Go API Server
```dockerfile
# Build stage
FROM golang:1.19-alpine AS builder
WORKDIR /app
COPY go.* ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -o api

# Production stage
FROM alpine:3.18
RUN apk add --no-cache ca-certificates
COPY --from=builder /app/api /usr/local/bin/
COPY --from=builder /app/configs /configs
EXPOSE 8080
CMD ["api", "--config", "/configs/prod.yaml"]
```

### Python FastAPI
```dockerfile
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

ENV PORT=8000
EXPOSE 8000

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

## Databases 💾

### PostgreSQL Custom
```dockerfile
FROM postgres:15-alpine

# Configurações customizadas
COPY postgresql.conf /etc/postgresql/postgresql.conf
COPY init.sql /docker-entrypoint-initdb.d/

# Timezone
ENV TZ=UTC
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# Volumes
VOLUME ["/var/lib/postgresql/data"]

EXPOSE 5432
```

### Redis with Custom Config
```dockerfile
FROM redis:7-alpine

# Copiar configuração customizada
COPY redis.conf /usr/local/etc/redis/redis.conf

# Persistência
VOLUME ["/data"]

EXPOSE 6379
CMD ["redis-server", "/usr/local/etc/redis/redis.conf"]
```

## CLI Tools 🛠️

### AWS CLI Tool
```dockerfile
FROM alpine:3.18

RUN apk add --no-cache \
    python3 \
    py3-pip \
    groff \
    less \
    && pip3 install --no-cache-dir awscli

WORKDIR /aws

ENTRYPOINT ["aws"]
```

### Custom DevOps Tool
```dockerfile
FROM ubuntu:22.04

# Ferramentas essenciais
RUN apt-get update && apt-get install -y \
    curl \
    wget \
    git \
    jq \
    vim \
    && rm -rf /var/lib/apt/lists/*

# Scripts customizados
COPY scripts/ /usr/local/bin/
RUN chmod +x /usr/local/bin/*

WORKDIR /workspace
ENTRYPOINT ["/bin/bash"]
```

## Microservices 🚀

### Java Spring Boot
```dockerfile
# Build stage
FROM eclipse-temurin:17-jdk-alpine AS builder
WORKDIR /app
COPY . .
RUN ./gradlew bootJar

# Production stage
FROM eclipse-temurin:17-jre-alpine
WORKDIR /app
COPY --from=builder /app/build/libs/*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
```

### Rust Microservice
```dockerfile
# Build stage
FROM rust:1.70 AS builder
WORKDIR /usr/src/app
COPY . .
RUN cargo build --release

# Production stage
FROM debian:bullseye-slim
RUN apt-get update && apt-get install -y \
    ca-certificates \
    && rm -rf /var/lib/apt/lists/*
COPY --from=builder /usr/src/app/target/release/myapp /usr/local/bin/
CMD ["myapp"]
```

## Development Environments 🔧

### PHP Development
```dockerfile
FROM php:8.2-apache

# Extensões PHP
RUN docker-php-ext-install \
    pdo_mysql \
    mysqli

# Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Apache config
COPY apache.conf /etc/apache2/sites-available/000-default.conf
RUN a2enmod rewrite

# App
WORKDIR /var/www/html
COPY . .
RUN composer install

EXPOSE 80
```

### Vue.js Development
```dockerfile
FROM node:18-alpine

WORKDIR /app

# Dependencies
COPY package*.json ./
RUN npm install

# Hot reload config
COPY . .
ENV HOST=0.0.0.0
EXPOSE 5173

CMD ["npm", "run", "dev"]
```

## Production Patterns 🏭

### Secure Python App
```dockerfile
FROM python:3.11-slim AS builder

WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .
RUN python -m compileall -b .

FROM python:3.11-slim
WORKDIR /app

# Usuário não-root
RUN useradd -m -r appuser && \
    chown appuser:appuser /app

# Apenas arquivos compilados
COPY --from=builder /app/*.pyc .
COPY --from=builder /app/configs ./configs

USER appuser
CMD ["python", "main.pyc"]
```

### Optimized Node.js
```dockerfile
FROM node:18-alpine AS builder

WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

FROM node:18-alpine
WORKDIR /app

# Apenas dependências de produção
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/package*.json ./
RUN npm ci --only=production

# Configurações de segurança
ENV NODE_ENV=production
USER node
EXPOSE 3000
CMD ["node", "dist/main.js"]
```

## Waifu Example Tips 💡

> **Build-chan diz:** "Multi-stage builds são como magia - reduzem o tamanho final e aumentam a segurança! ✨"
{style="tip"}

> **Security-chan alerta:** "Sempre use versões específicas das imagens base! `latest` é perigoso! 🛡️"
{style="warning"}

> **Performance-chan sugere:** "Organize as camadas do menos mutável para o mais mutável! 🚀"
{style="note"}

## Checkpoint ✅

Você agora tem exemplos de:
- [x] Aplicações Web modernas
- [x] Backends e APIs
- [x] Bancos de dados customizados
- [x] Ferramentas CLI
- [x] Microserviços
- [x] Ambientes de desenvolvimento
- [x] Configurações de produção

## Próximos Passos 🎯

1. [Best Practices](best-practices.md)
2. [Advanced Patterns](advanced-patterns.md)
3. [Production Deployment](production-deployment.md)

## Recursos Adicionais 📚

- [Docker Official Images](https://hub.docker.com/search?q=&type=image&image_filter=official)
- [Dockerfile Best Practices](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)
- [Docker Compose Examples](https://github.com/docker/awesome-compose)

> **Sensei's Note:** "Um bom exemplo vale mais que mil explicações. Estude, adapte e crie seus próprios padrões! 🎯"
{style="note"}