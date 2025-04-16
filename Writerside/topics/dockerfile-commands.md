# Dockerfile Commands: Guia Definitivo 🛠️

```ascii
╔═══════════════════════════════════════════════════════════╗
║ DOCKERFILE COMMAND REFERENCE                              ║
║                                                          ║
║ FROM │ ARG │ ENV │ WORKDIR │ COPY │ ADD │ RUN │ CMD     ║
║                                                          ║
║ ENTRYPOINT │ EXPOSE │ VOLUME │ USER │ LABEL │ ONBUILD   ║
╚═══════════════════════════════════════════════════════════╝
```

## Comandos Base 📝

### FROM
```dockerfile
# Sintaxe básica
FROM <imagem>
FROM <imagem>:<tag>
FROM <imagem>@<digest>

# Exemplos
FROM ubuntu:22.04
FROM node:18-alpine
FROM golang:1.19@sha256:8f2b0...
```

> **From-chan diz:** "Sempre especifique uma tag ou digest! `latest` é perigoso em produção! 🎯"
{style="warning"}

### ARG
```dockerfile
# Sintaxe básica
ARG <nome>[=<default>]

# Exemplos
ARG VERSION=1.0.0
ARG BUILD_DATE
ARG GITHUB_TOKEN  # Sensível - usar com --build-arg

# Escopo especial
ARG BASE_IMAGE=ubuntu:22.04
FROM ${BASE_IMAGE}
```

### ENV
```dockerfile
# Sintaxe básica
ENV <key>=<value>
ENV <key> <value>

# Exemplos
ENV NODE_ENV=production
ENV PORT=3000 \
    HOST=0.0.0.0 \
    APP_NAME="Docker App"
```

### WORKDIR
```dockerfile
# Sintaxe básica
WORKDIR /path/to/dir

# Exemplos
WORKDIR /app
WORKDIR $HOME/app
WORKDIR src  # Relativo ao WORKDIR anterior
```

## Comandos de Conteúdo 📦

### COPY
```dockerfile
# Sintaxe básica
COPY <src> <dest>
COPY ["<src>",... "<dest>"]

# Exemplos
COPY . .
COPY package*.json ./
COPY --chown=node:node . .
COPY --from=builder /app/dist ./public
```

### ADD
```dockerfile
# Sintaxe básica
ADD <src> <dest>
ADD ["<src>",... "<dest>"]

# Exemplos
ADD https://example.com/file.tar.gz /tmp/
ADD source.tar.gz /usr/local/
ADD --chown=user:group files* /app/
```

> **Copy-chan avisa:** "Prefira COPY ao invés de ADD! ADD tem poderes especiais que podem ser perigosos! 🎭"
{style="warning"}

## Comandos de Execução 🚀

### RUN
```dockerfile
# Sintaxe shell
RUN <comando>

# Sintaxe exec
RUN ["executável", "param1", "param2"]

# Exemplos práticos
RUN apt-get update && \
    apt-get install -y \
        curl \
        nginx \
    && rm -rf /var/lib/apt/lists/*

RUN --mount=type=cache,target=/var/cache/apt \
    apt-get update && apt-get install -y python3

RUN --mount=type=secret,id=ssh,target=/root/.ssh/id_rsa \
    git clone git@github.com:org/repo.git
```

### CMD
```dockerfile
# Sintaxe básica
CMD ["executável", "param1", "param2"]  # exec form (preferida)
CMD comando param1 param2               # shell form
CMD ["param1", "param2"]               # como parâmetros para ENTRYPOINT

# Exemplos
CMD ["nginx", "-g", "daemon off;"]
CMD ["python3", "app.py"]
CMD ["--port", "8080"]  # com ENTRYPOINT
```

### ENTRYPOINT
```dockerfile
# Sintaxe básica
ENTRYPOINT ["executável", "param1"]  # exec form (preferida)
ENTRYPOINT comando param1            # shell form

# Exemplos
ENTRYPOINT ["nginx", "-g", "daemon off;"]
ENTRYPOINT ["/docker-entrypoint.sh"]
ENTRYPOINT ["python3"]
```

## Comandos de Configuração ⚙️

### EXPOSE
```dockerfile
# Sintaxe básica
EXPOSE <port> [<port>/<protocol>...]

# Exemplos
EXPOSE 80
EXPOSE 80/tcp 443/tcp
EXPOSE 53/udp
```

### VOLUME
```dockerfile
# Sintaxe básica
VOLUME ["/data"]
VOLUME /data /logs /temp

# Exemplos
VOLUME ["/var/log"]
VOLUME ["/var/www", "/var/log/nginx"]
VOLUME /mnt/data
```

### USER
```dockerfile
# Sintaxe básica
USER <user>[:<group>]
USER <UID>[:<GID>]

# Exemplos
USER node
USER node:node
USER 1000:1000
```

### LABEL
```dockerfile
# Sintaxe básica
LABEL <key>=<value>

# Exemplos
LABEL version="1.0.0"
LABEL maintainer="dev@example.com" \
      description="Nginx server" \
      vendor="ACME Corp"
```

## Comandos Avançados 🔧

### HEALTHCHECK
```dockerfile
# Sintaxe básica
HEALTHCHECK [OPTIONS] CMD comando

# Exemplos
HEALTHCHECK --interval=30s --timeout=3s \
    CMD curl -f http://localhost/ || exit 1

HEALTHCHECK --start-period=5s \
    CMD wget -q --spider http://localhost:3000/health || exit 1

# Desabilitar healthcheck
HEALTHCHECK NONE
```

### SHELL
```dockerfile
# Sintaxe básica
SHELL ["executável", "parâmetros"]

# Exemplos
SHELL ["/bin/bash", "-c"]
SHELL ["powershell", "-command"]
```

### ONBUILD
```dockerfile
# Sintaxe básica
ONBUILD <INSTRUÇÃO>

# Exemplos
ONBUILD COPY . /app/src
ONBUILD RUN /usr/local/bin/python-build
```

### STOPSIGNAL
```dockerfile
# Sintaxe básica
STOPSIGNAL signal

# Exemplos
STOPSIGNAL SIGTERM
STOPSIGNAL 9
```

## Padrões e Truques 🎯

### Multi-stage Builds
```dockerfile
# Build stage
FROM golang:1.19 AS builder
WORKDIR /app
COPY . .
RUN go build -o main

# Final stage
FROM alpine:3.18
COPY --from=builder /app/main /usr/local/bin/
ENTRYPOINT ["main"]
```

### Build Cache
```dockerfile
# Cache de dependências
COPY go.mod go.sum ./
RUN go mod download

# Cache de build
RUN --mount=type=cache,target=/root/.cache/go-build \
    go build -o app
```

### Args e Env Combinados
```dockerfile
# Build-time config
ARG NODE_ENV
ENV NODE_ENV=${NODE_ENV:-production}

# Runtime config
ARG CONFIG_FILE=config.json
COPY ${CONFIG_FILE} /etc/app/config.json
```

## Waifu Command Tips 💡

> **Build-chan diz:** "Mantenha suas instruções organizadas - do menos mutável para o mais mutável! 📚"
{style="tip"}

> **Cache-sama alerta:** "Use `--mount=type=cache` para builds mais rápidos! 🚀"
{style="warning"}

> **Security-chan lembra:** "Sempre use a forma exec para CMD e ENTRYPOINT em produção! 🔒"
{style="note"}

## Matriz de Compatibilidade 📊

| Comando | Múltiplos por Dockerfile | Sobrescrito por Args | Persiste na Imagem |
|---------|-------------------------|---------------------|-------------------|
| FROM    | ✅ (multi-stage)        | ✅                   | -                 |
| ARG     | ✅                       | ✅                   | ❌                |
| ENV     | ✅                       | ❌                   | ✅                |
| COPY    | ✅                       | ✅ (src/dest)        | ✅                |
| RUN     | ✅                       | ❌                   | ✅                |
| CMD     | ❌ (último vale)         | ❌                   | ✅                |

## Checkpoint ✅

Você agora domina:
- [x] Todos os comandos básicos do Dockerfile
- [x] Sintaxe e opções de cada comando
- [x] Casos de uso e exemplos práticos
- [x] Padrões avançados e truques
- [x] Boas práticas para cada comando
- [x] Compatibilidade e limitações

## Próximos Passos 🎯

1. [Dockerfile Examples](dockerfile-examples.md)
2. [Best Practices](best-practices.md)
3. [Advanced Patterns](advanced-patterns.md)

## Recursos Adicionais 📚

- [Dockerfile Reference](https://docs.docker.com/engine/reference/builder/)
- [Best Practices Guide](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)
- [Multi-stage Builds](https://docs.docker.com/build/building/multi-stage/)

> **Sensei's Note:** "Cada comando é uma ferramenta - aprenda quando e como usar cada uma delas para construir imagens perfeitas! 🎯"
{style="note"}