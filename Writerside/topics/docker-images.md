# Docker Images: Blueprint do Seu Container 🎨

```ascii
╔═══════════════════════════════════════════════════════════╗
║ IMAGE ARCHITECTURE MATRIX                                 ║
║                                                          ║
║    Base Image Layer        [Alpine/Ubuntu/Debian]        ║
║         ↓                                                ║
║    Dependencies Layer      [Runtime/Libraries]           ║
║         ↓                                                ║
║    Application Layer       [Your Code/Apps]              ║
║         ↓                                                ║
║    Configuration Layer     [ENV/Settings]                ║
║         ↓                                                ║
║    Runtime Layer          [CMD/ENTRYPOINT]              ║
╚═══════════════════════════════════════════════════════════╝
```

## Fundamentos de Imagens 📚

### Conceitos Básicos
- **Imagem**: Template read-only para containers
- **Container**: Instância executável de uma imagem
- **Layer**: Camada incremental de mudanças
- **Registry**: Repositório de imagens
- **Tag**: Identificador de versão
- **Digest**: Hash SHA256 único da imagem

### Tipos de Imagens
1. **Base Images**
   - Scratch
   - Alpine (5MB)
   - Debian Slim (80MB)
   - Ubuntu (120MB)

2. **Runtime Images**
   - node:alpine
   - python:slim
   - openjdk:slim

3. **Application Images**
   - nginx:alpine
   - postgres:alpine
   - redis:alpine

## Anatomia Detalhada 🔬

### Sistema de Camadas
```bash
# Análise de camadas
docker history --no-trunc nginx:latest

# Metadata detalhada
docker inspect --format='{{.RootFS.Layers}}' nginx:latest

# Exportar imagem
docker save nginx:latest | tar -tvf -
```

### Estrutura Interna
```dockerfile
# Exemplo de construção em camadas
FROM alpine:3.14
# Layer 1: Base OS

RUN apk add --no-cache python3
# Layer 2: Runtime

COPY ./app /app
# Layer 3: Application

ENV APP_ENV=production
# Layer 4: Configuration

CMD ["python3", "/app/main.py"]
# Layer 5: Runtime command
```

## Gerenciamento Avançado 🛠️

### Image Operations
```bash
# Criar tag local
docker tag source:latest target:v1.0

# Salvar imagem como tar
docker save myapp:latest > myapp.tar

# Carregar imagem de tar
docker load < myapp.tar

# Filtrar imagens
docker images --filter "dangling=true"
docker images --filter "label=environment=prod"
```

### Batch Operations
```bash
# Remover todas imagens não utilizadas
docker image prune --all --force

# Remover por padrão
docker images -q "python*" | xargs docker rmi

# Limpar imagens antigas
docker images --format "{{.ID}}\t{{.CreatedAt}}" | sort -k 2 | head -n 5 | cut -f1 | xargs docker rmi
```

## Sistema de Tags 🏷️

### Convenções de Versionamento
```bash
# Tags específicas
registry.example.com/app:1.0.0
username/app:latest
custom/app:dev-build

# Multi-arquitetura
docker pull --platform linux/amd64 nginx
docker pull --platform linux/arm64 nginx
```

## Boas Práticas 📝

### DO's ✅
- Use tags específicas
- Minimize camadas
- Otimize cache
- Documente dependências
- Implemente multi-stage builds

### DON'Ts ❌
- Evite `latest` em produção
- Não armazene secrets
- Não instale pacotes desnecessários
- Não ignore `.dockerignore`
- Não use imagens não oficiais sem verificação

## Image Operations Matrix 📊

| Operação | Comando | Uso Comum |
|----------|---------|-----------|
| Build | `docker build` | Criar nova imagem |
| Pull | `docker pull` | Baixar do registry |
| Push | `docker push` | Enviar para registry |
| Tag | `docker tag` | Criar alias/versão |
| Remove | `docker rmi` | Deletar imagem |
| Inspect | `docker inspect` | Ver metadata |
| Prune | `docker image prune` | Limpar não usadas |

## Otimização de Imagens 🚀

### Redução de Tamanho
```dockerfile
# Multi-stage build
FROM node:alpine AS builder
WORKDIR /app
COPY . .
RUN npm ci && npm run build

FROM nginx:alpine
COPY --from=builder /app/dist /usr/share/nginx/html
```

### Cache Optimization
```dockerfile
# Ordem eficiente
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build
```

## Registry Management 🌐

### Docker Hub
```bash
# Login
docker login [options] [SERVER]

# Logout
docker logout [SERVER]

# Pull de registry privado
docker pull private-registry.com/app:tag
```

## Advanced Security 🔒

### Image Signing
```bash
# Configurar DCT (Docker Content Trust)
export DOCKER_CONTENT_TRUST=1

# Gerar chaves
docker trust key generate mykey

# Assinar imagem
docker trust sign myregistry/myimage:tag

# Verificar assinatura
docker trust inspect --pretty myregistry/myimage:tag
```

### Security Scanning
```bash
# Trivy scan
trivy image python:3.9-alpine

# Snyk scan
snyk container test nginx:latest

# Anchore scan
anchore-cli image add nginx:latest
anchore-cli image wait nginx:latest
anchore-cli image vuln nginx:latest os
```

## Monitoring & Metrics 📊

### Image Analytics
```bash
# Tamanho das camadas
docker history --no-trunc --format "{{.Size}}\t{{.CreatedBy}}" nginx:latest

# Uso de disco
docker system df -v --format "{{.Type}}\t{{.TotalCount}}\t{{.Size}}"

# Cache status
docker builder prune -f --filter until=24h
```

### Performance Tracking
```bash
# Build time analysis
time docker build --no-cache .

# Layer size tracking
docker image inspect myapp:latest -f '{{.RootFS.Layers}}'

# Pull time metrics
time docker pull large-image:latest
```

## Advanced Troubleshooting 🔧

### Debug Techniques
```bash
# Debug build
DOCKER_BUILDKIT=1 docker build --progress=plain .

# Layer inspection
docker save myimage:latest | tar -xC /tmp/image-layers

# Network issues
docker pull --verbose registry.example.com/image:tag
```

### Common Issues Matrix

| Problema | Diagnóstico | Solução |
|----------|-------------|---------|
| Pull Timeout | `docker pull -v` | Verificar rede/proxy |
| Build Failure | `--no-cache` | Limpar cache/deps |
| Layer Issues | `docker history` | Otimizar Dockerfile |
| Space Issues | `docker system df` | Prune/cleanup |

## Automation & CI/CD 🤖

### GitHub Actions
```yaml
name: Docker Build

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      
      - name: Set up QEMU
        uses: docker/setup-qemu-action@v1
      
      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v1
      
      - name: Login to DockerHub
        uses: docker/login-action@v1
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}
      
      - name: Build and push
        uses: docker/build-push-action@v2
        with:
          context: .
          platforms: linux/amd64,linux/arm64
          push: true
          tags: user/app:latest
```

### Automated Testing
```bash
# Test script
#!/bin/bash
set -e

# Build image
docker build -t test-image .

# Run container tests
docker run --rm test-image npm test

# Security scan
trivy image test-image

# Cleanup
docker rmi test-image
```

## Waifu Advanced Tips 💫

> **Registry-chan diz:** "Mantenha seus registries organizados como uma coleção de mangás! 📚"
{style="tip"}

> **Build-sama alerta:** "Multi-stage builds são como transformações de magical girls - mais leves e poderosas! ✨"
{style="warning"}

> **Cache-kun ensina:** "Cache é como um bento box - organize bem para reutilizar depois! 🍱"
{style="note"}

## Quick Reference Pro 📋

```ascii
╔════════════════════════════════════════════════════╗
║ ADVANCED IMAGE MANAGEMENT                          ║
╠════════════════════════════════════════════════════╣
║ Multi-arch │ docker buildx build --platform all    ║
║ Sign       │ docker trust sign image:tag          ║
║ Scan       │ trivy image nginx:latest             ║
║ Analyze    │ docker history --no-trunc image      ║
║ Debug      │ DOCKER_BUILDKIT=1 docker build .     ║
║ Optimize   │ docker build --squash .              ║
║ Monitor    │ docker events --filter type=image    ║
╚════════════════════════════════════════════════════╝
```

## Advanced Checkpoint ✅

Você agora domina:
- [x] Arquitetura interna de imagens
- [x] Multi-stage builds avançados
- [x] Registry privado e autenticação
- [x] Signing e scanning
- [x] Debug e troubleshooting
- [x] CI/CD integration
- [x] Performance optimization

## Next Level Steps 🎯

1. [Container Orchestration](kubernetes-intro.md)
2. [Advanced Security](security-hardening.md)
3. [Custom Base Images](base-image-creation.md)
4. [Registry Federation](registry-federation.md)
5. [Image Policy Management](image-policies.md)

> **Sensei's Final Note:** "Uma imagem Docker é como um kata perfeito - cada movimento tem propósito, precisão e eficiência! 🥋"
{style="note"}