# Docker Bind Mounts: Host Integration 🔗

```ascii
╔═══════════════════════════════════════╗
║           BIND MOUNT FLOW             ║
║                                       ║
║    [Host Dir] ←→ [Mount Point] ←→    ║
║                     ↓                 ║
║              [Container]              ║
║                                       ║
╚═══════════════════════════════════════╝
```

## Basic Usage 📝

### Mount Syntax
```bash
# Sintaxe longa (recomendada)
docker run \
  --mount type=bind,source=/host/path,target=/container/path \
  nginx

# Sintaxe curta
docker run -v /host/path:/container/path nginx

# Modo somente leitura
docker run \
  --mount type=bind,source=/config,target=/etc/nginx,readonly \
  nginx
```

### Common Use Cases
```bash
# Desenvolvimento local
docker run \
  --mount type=bind,source=$(pwd),target=/app \
  node:alpine npm run dev

# Configuração externa
docker run \
  --mount type=bind,source=/etc/configs,target=/etc/app,readonly \
  app:latest

# Log persistente
docker run \
  --mount type=bind,source=/var/log/app,target=/var/log/container \
  app:latest
```

## Permission Management 🔒

### User Mapping
```bash
# Definir usuário específico
docker run \
  --mount type=bind,source=/data,target=/app/data \
  --user $(id -u):$(id -g) \
  app

# Permissões específicas
docker run \
  --mount type=bind,source=/data,target=/app/data \
  -e PUID=1000 -e PGID=1000 \
  app
```

### SELinux Context
```bash
# Adicionar contexto z
docker run \
  --mount type=bind,source=/data,target=/app/data,z \
  app

# Adicionar contexto Z (privado)
docker run \
  --mount type=bind,source=/data,target=/app/data,Z \
  app
```

## Development Workflow 🛠️

### Hot Reload Setup
```yaml
version: '3.8'
services:
  web:
    image: node:alpine
    command: npm run dev
    working_dir: /app
    ports:
      - "3000:3000"
    volumes:
      - type: bind
        source: ./src
        target: /app/src
      - type: bind
        source: ./package.json
        target: /app/package.json
```

### Multi-mount Configuration
```yaml
services:
  app:
    build: .
    volumes:
      - type: bind
        source: ./src
        target: /app/src
      - type: bind
        source: ./config
        target: /app/config
        read_only: true
      - type: bind
        source: ./logs
        target: /app/logs
```

## Performance Considerations 🚀

### Best Practices
1. Minimize bind mount uso em produção
2. Use caching para node_modules
3. Considere volume para dados dinâmicos
4. Monte apenas diretórios necessários
5. Evite bind mounts recursivos grandes

### Cache Configuration
```yaml
services:
  web:
    volumes:
      # Código fonte
      - type: bind
        source: ./src
        target: /app/src
      # Cache de dependências
      - type: volume
        source: node_modules
        target: /app/node_modules

volumes:
  node_modules:
```

## Troubleshooting 🔍

### Common Issues
```bash
# Verificar permissões
ls -la /host/path

# Testar acesso
docker run --rm \
  --mount type=bind,source=/host/path,target=/test \
  alpine ls -la /test

# Verificar SELinux
ls -Z /host/path
```

### Debug Commands
```bash
# Listar montagens
docker inspect -f '{{range .Mounts}}{{.Type}} {{.Source}} -> {{.Destination}}{{"\n"}}{{end}}' container

# Verificar logs
docker logs container 2>&1 | grep "permission denied"

# Testar permissões
docker run --rm \
  --mount type=bind,source=/host/path,target=/test \
  alpine touch /test/testfile
```

## Security Considerations 🛡️

### Access Control
```bash
# Limitar escopo
docker run \
  --mount type=bind,source=/specific/path,target=/data,readonly \
  --cap-drop=ALL \
  app

# Isolamento de rede
docker run \
  --mount type=bind,source=/config,target=/app/config \
  --network none \
  app
```

### Risk Mitigation
1. Use modo somente leitura quando possível
2. Limite o escopo dos bind mounts
3. Verifique permissões do host
4. Implemente user namespaces
5. Monitore atividade do bind mount

## Docker Compose Integration 📋

### Development Setup
```yaml
version: '3.8'
services:
  app:
    build: .
    volumes:
      - type: bind
        source: ./src
        target: /app/src
      - type: bind
        source: ./config
        target: /app/config
        read_only: true
    environment:
      - NODE_ENV=development
    command: npm run dev
```

### Production Setup
```yaml
version: '3.8'
services:
  app:
    image: app:prod
    volumes:
      - type: bind
        source: /etc/app/config
        target: /app/config
        read_only: true
      - type: bind
        source: /var/log/app
        target: /app/logs
    restart: unless-stopped
```

## Tips & Tricks 💡

> **Mount-san diz:** "Use bind mounts para desenvolvimento, volumes para produção! 🎯"
{style="tip"}

> **Security-chan alerta:** "Sempre verifique as permissões do host! ⚠️"
{style="warning"}

> **Debug-kun sugere:** "Monitore os logs para problemas de montagem! 🔍"
{style="note"}

## Próximos Passos 🎯

1. [Docker Volumes](docker-volumes.md)
2. [Container Storage](container-storage.md)
3. [Development Workflow](development-workflow.md)

## Referências 📚

- [Bind Mounts Overview](https://docs.docker.com/storage/bind-mounts/)
- [Docker Development Guide](https://docs.docker.com/develop/)
- [Security Best Practices](https://docs.docker.com/engine/security/)
- [Docker Compose File Reference](https://docs.docker.com/compose/compose-file/)

> **Bind Mount Master's Note:** "Bind mounts são a ponte entre seu ambiente de desenvolvimento e containers - use com sabedoria! 🌉"
{style="note"}