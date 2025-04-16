# Dockerfile: A Arte de Construir Imagens 🎨

```ascii
╔════════════════════════════════════════════════════════════╗
║ DOCKERFILE STRUCTURE                                       ║
║                                                           ║
║ FROM base_image                                           ║
║ ├── ENV variables                                         ║
║ ├── COPY/ADD files                                        ║
║ ├── RUN commands                                          ║
║ ├── EXPOSE ports                                          ║
║ └── CMD/ENTRYPOINT                                        ║
║                                                           ║
╚════════════════════════════════════════════════════════════╝
```

## O que é um Dockerfile? 🤔

Um Dockerfile é um arquivo de texto que contém todas as instruções necessárias para criar uma imagem Docker. 
Pense nele como uma receita que o Docker usa para "cozinhar" sua aplicação em um container.

### Características Principais
- Formato texto simples
- Execução sequencial de instruções
- Sistema de camadas (layers)
- Cache inteligente
- Sintaxe declarativa

### Por que usar Dockerfile?
- Automação da criação de imagens
- Reprodutibilidade
- Versionamento
- Padronização
- Facilidade de compartilhamento

## Anatomia Básica 🔬

### Estrutura Fundamental
```dockerfile
# Comentário explicativo
FROM ubuntu:20.04

# Variáveis de ambiente
ENV APP_HOME=/app \
    APP_ENV=production

# Diretório de trabalho
WORKDIR $APP_HOME

# Copiar arquivos
COPY . .

# Executar comandos
RUN apt-get update && \
    apt-get install -y python3

# Expor portas
EXPOSE 8080

# Comando padrão
CMD ["python3", "app.py"]
```

## Instruções Essenciais 📝

### FROM
A instrução FROM é sempre a primeira instrução válida em um Dockerfile.

```dockerfile
# Imagem base oficial
FROM node:18-alpine

# Imagem scratch (vazia)
FROM scratch

# Multi-stage build
FROM golang:1.19 AS builder
```

> **Build-chan diz:** "A escolha da imagem base é como escolher o terreno para construir sua casa - precisa ser sólido! 🏗️"
{style="tip"}

### ARG
ARG é a única instrução que pode vir antes de FROM.

```dockerfile
# Definir versão durante o build
ARG NODE_VERSION=18
FROM node:${NODE_VERSION}-alpine

# Múltiplos argumentos
ARG BUILD_ENV
ARG BUILD_VERSION
```

### ENV
Variáveis de ambiente que persistem na imagem final.

```dockerfile
# Variáveis simples
ENV APP_HOME=/app

# Múltiplas variáveis
ENV APP_ENV=production \
    APP_PORT=3000 \
    APP_VERSION=1.0.0

# Usando variáveis
ENV PATH="${APP_HOME}/bin:${PATH}"
```

### WORKDIR
Define o diretório de trabalho para instruções subsequentes.

```dockerfile
# Diretório absoluto
WORKDIR /app

# Diretório relativo (concatena com WORKDIR anterior)
WORKDIR src
WORKDIR tests  # Resultado: /app/src/tests
```

### COPY e ADD
```dockerfile
# Copiar arquivo local
COPY package.json .

# Copiar com wildcard
COPY *.js ./

# Copiar preservando permissões
COPY --chown=node:node . .

# ADD para URLs e archives
ADD https://example.com/file.tar.gz .
ADD source.tar.gz /usr/local/
```

### RUN
```dockerfile
# Formato shell
RUN apt-get update

# Formato exec
RUN ["apt-get", "install", "-y", "nginx"]

# Multi-line com limpeza
RUN apt-get update && \
    apt-get install -y \
    curl \
    nginx \
    postgresql-client \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get clean

# Criar usuário e grupo
RUN groupadd -r appgroup && \
    useradd -r -g appgroup appuser
```

### EXPOSE
```dockerfile
# Expor uma porta
EXPOSE 8080

# Expor múltiplas portas
EXPOSE 80 443

# Expor UDP
EXPOSE 53/udp
```

### CMD e ENTRYPOINT
```dockerfile
# CMD formato exec (recomendado)
CMD ["nginx", "-g", "daemon off;"]

# CMD formato shell (evitar)
CMD nginx -g "daemon off;"

# ENTRYPOINT com CMD
ENTRYPOINT ["python3"]
CMD ["app.py"]

# Script de entrada
COPY docker-entrypoint.sh /
ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["app", "start"]
```

## Boas Práticas Avançadas 🌟

### 1. Otimização de Camadas
```dockerfile
# ❌ Ruim - Muitas camadas
RUN apt-get update
RUN apt-get install -y python3
RUN apt-get install -y nginx
RUN rm -rf /var/lib/apt/lists/*

# ✅ Bom - Uma única camada
RUN apt-get update && \
    apt-get install -y \
    python3 \
    nginx \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get clean
```

### 2. Cache Eficiente
```dockerfile
# ✅ Copiar apenas arquivos de dependência primeiro
COPY package*.json ./
RUN npm ci

# Copiar código-fonte depois
COPY . .
RUN npm run build

# ✅ Agrupar comandos relacionados
RUN curl -sL https://example.com/setup.sh | bash - && \
    apt-get update && \
    apt-get install -y package1 package2 && \
    rm -rf /var/lib/apt/lists/*
```

### 3. Multi-stage Builds Avançados
```dockerfile
# Stage de build
FROM node:18 AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# Stage de testes
FROM builder AS tester
RUN npm run test

# Stage final
FROM nginx:alpine
COPY --from=builder /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf
```

### 4. Segurança
```dockerfile
# Usar versões específicas
FROM node:18.17.1-alpine3.18

# Criar e usar usuário não-root
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser

# Minimizar superfície de ataque
FROM alpine:3.18
RUN apk add --no-cache nodejs
```

## Padrões Avançados 🎯

### 1. Imagens Base Personalizadas
```dockerfile
# Dockerfile.base
FROM ubuntu:22.04
RUN apt-get update && \
    apt-get install -y \
    curl \
    nginx \
    && rm -rf /var/lib/apt/lists/*

# Dockerfile
FROM myorg/baseimage:1.0
COPY . .
```

### 2. Build Arguments Dinâmicos
```dockerfile
# Build-time variables
ARG BUILD_VERSION
ARG BUILD_DATE
ARG VCS_REF

# Labels para metadata
LABEL org.label-schema.build-date=$BUILD_DATE \
      org.label-schema.vcs-ref=$VCS_REF \
      org.label-schema.version=$BUILD_VERSION
```

### 3. Health Checks
```dockerfile
# Verificação básica
HEALTHCHECK CMD curl -f http://localhost/ || exit 1

# Verificação avançada
HEALTHCHECK --interval=30s --timeout=3s \
    CMD wget -q --spider http://localhost:3000/health || exit 1
```

## Waifu Docker Tips 💡

> **Dockerfile-chan diz:** "Mantenha suas imagens leves e eficientes! Menos é mais! 🎨"
{style="tip"}

> **Cache-sama alerta:** "Organize suas instruções da menos mutável para a mais mutável! 🔄"
{style="warning"}

> **Security-chan lembra:** "Nunca exponha secrets no Dockerfile! Use build args ou environment files! 🔒"
{style="note"}

> **Performance-chan sugere:** "Use multi-stage builds para reduzir o tamanho final da imagem! 🚀"
{style="tip"}

## Exemplos do Mundo Real 🌍

### Aplicação Web Full-stack
```dockerfile
# Build stage
FROM node:18-alpine AS frontend-builder
WORKDIR /app
COPY frontend/package*.json ./
RUN npm ci
COPY frontend/ .
RUN npm run build

# Backend stage
FROM golang:1.19-alpine AS backend-builder
WORKDIR /app
COPY backend/go.* ./
RUN go mod download
COPY backend/ .
RUN CGO_ENABLED=0 go build -o api

# Final stage
FROM alpine:3.18
RUN apk add --no-cache ca-certificates nginx
COPY --from=frontend-builder /app/dist /usr/share/nginx/html
COPY --from=backend-builder /app/api /usr/local/bin/
COPY nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

### Microserviço com Cache
```dockerfile
FROM golang:1.19-alpine AS builder

# Cache go modules
COPY go.mod go.sum ./
RUN go mod download

# Build app with version info
ARG VERSION
ARG BUILD_DATE
RUN --mount=type=cache,target=/root/.cache/go-build \
    go build -ldflags="-X main.Version=${VERSION} -X main.BuildDate=${BUILD_DATE}" \
    -o /app

FROM alpine:3.18
COPY --from=builder /app /usr/local/bin/
ENTRYPOINT ["/usr/local/bin/app"]
```

## Checkpoint ✅

Você agora domina:
- [x] Estrutura básica do Dockerfile
- [x] Instruções principais e seus usos
- [x] Boas práticas e otimizações
- [x] Multi-stage builds avançados
- [x] Otimização de cache
- [x] Segurança e melhores práticas
- [x] Padrões avançados de build
- [x] Exemplos do mundo real

## Próximos Passos 🎯

1. [Dockerfile Commands](dockerfile-commands.md) - Aprofunde-se nas instruções
2. [Dockerfile Examples](dockerfile-examples.md) - Veja mais exemplos práticos
3. [Best Practices](best-practices.md) - Domine as melhores práticas

## Recursos Adicionais 📚

- [Dockerfile Reference](https://docs.docker.com/engine/reference/builder/)
- [Docker Build Guide](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)
- [Multi-stage Builds](https://docs.docker.com/develop/develop-images/multistage-build/)

> **Sensei's Note:** "Um Dockerfile é como uma receita - os ingredientes importam, mas a ordem e o método fazem toda a diferença! Pratique, refine e sempre busque a excelência em suas builds! 🎯"
{style="note"}