# Docker Compose File: Anatomia Completa 📑

```ascii
╔════════════════════════════════════════════════════════════╗
║ DOCKER COMPOSE FILE REFERENCE                              ║
║                                                           ║
║ Version │ Services │ Networks │ Volumes │ Configs │ Deploy ║
║                                                           ║
║ Basic → Advanced → Production → Swarm                      ║
╚════════════════════════════════════════════════════════════╝
```

## Estrutura Base 📋

### Versões Suportadas
```yaml
version: "3.8"  # Última versão estável
# ou
version: "2.4"  # Legacy support
```

### Componentes Principais
```yaml
version: "3.8"
services:     # Definição dos containers
networks:     # Configuração de redes
volumes:      # Volumes persistentes
configs:      # Configurações
secrets:      # Dados sensíveis
```

## Services Deep Dive 🔍

### Configurações Básicas
```yaml
services:
  webapp:
    image: nginx:alpine          # Imagem base
    build: ./webapp             # Ou build local
    container_name: myapp       # Nome personalizado
    hostname: webapp           # Hostname interno
    ports:
      - "80:80"               # Port mapping
    environment:              # Variáveis de ambiente
      NODE_ENV: production
```

### Configurações Avançadas
```yaml
services:
  api:
    build:
      context: ./api
      dockerfile: Dockerfile.prod
      args:
        BUILD_ENV: production
    deploy:
      replicas: 3
      resources:
        limits:
          cpus: '0.50'
          memory: 512M
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost"]
      interval: 30s
      timeout: 10s
      retries: 3
```

## Network Configuration 🌐

### Tipos de Rede
```yaml
networks:
  frontend:
    driver: bridge
    
  backend:
    driver: overlay
    internal: true
    
  custom:
    driver: macvlan
    driver_opts:
      parent: eth0
```

### Service Network Config
```yaml
services:
  web:
    networks:
      frontend:
        ipv4_address: 172.16.1.10
      backend:
        aliases:
          - webapp
```

## Volume Configuration 💾

### Tipos de Volume
```yaml
volumes:
  db_data:    # Named volume
    driver: local
    
  cache:
    driver: tmpfs
    driver_opts:
      size: 100M
```

### Service Volume Config
```yaml
services:
  db:
    volumes:
      - db_data:/var/lib/mysql     # Named volume
      - ./config:/etc/mysql/conf.d  # Bind mount
      - /tmp/data:/tmp/data        # Host path
```

## Configs e Secrets 🔒

### Configurações
```yaml
configs:
  http_config:
    file: ./http.conf
  api_config:
    external: true

services:
  web:
    configs:
      - source: http_config
        target: /etc/nginx/conf.d/default.conf
```

### Secrets
```yaml
secrets:
  db_password:
    file: ./db_password.txt
  api_key:
    external: true

services:
  api:
    secrets:
      - db_password
      - api_key
```

## Deploy Configuration 🚀

### Swarm Mode
```yaml
services:
  api:
    deploy:
      mode: replicated
      replicas: 3
      placement:
        constraints:
          - node.role == worker
      update_config:
        parallelism: 2
        delay: 10s
      restart_policy:
        condition: on-failure
```

## Exemplos Completos 📚

### Stack de Desenvolvimento
```yaml
version: "3.8"

services:
  frontend:
    build: 
      context: ./frontend
      target: development
    volumes:
      - ./frontend:/app
      - /app/node_modules
    ports:
      - "3000:3000"
    environment:
      - REACT_APP_API_URL=http://api:5000
    depends_on:
      - api

  api:
    build: ./backend
    volumes:
      - ./backend:/app
    ports:
      - "5000:5000"
    environment:
      - DATABASE_URL=postgres://user:pass@db:5432/dbname
      - REDIS_URL=redis://cache:6379
    depends_on:
      - db
      - cache

  db:
    image: postgres:15-alpine
    environment:
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=pass
      - POSTGRES_DB=dbname
    volumes:
      - pgdata:/var/lib/postgresql/data

  cache:
    image: redis:7-alpine
    volumes:
      - redisdata:/data

volumes:
  pgdata:
  redisdata:

networks:
  default:
    driver: bridge
```

### Stack de Produção
```yaml
version: "3.8"

services:
  app:
    image: myapp:${TAG:-latest}
    deploy:
      replicas: 3
      resources:
        limits:
          cpus: '0.50'
          memory: 512M
      restart_policy:
        condition: on-failure
    configs:
      - source: app_config
        target: /app/config.yml
    secrets:
      - source: app_secret
        target: /app/secret.key
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost/health"]
      interval: 30s
      timeout: 10s
      retries: 3

configs:
  app_config:
    file: ./config/production.yml

secrets:
  app_secret:
    file: ./secrets/production.key
```

## Waifu Compose Tips 💡

> **Config-chan diz:** "Mantenha suas configurações organizadas e versionadas! 📝"
{style="tip"}

> **Deploy-chan alerta:** "Sempre teste suas configurações antes de ir para produção! 🚨"
{style="warning"}

> **Secret-chan lembra:** "Nunca comite seus secrets no repositório! 🔐"
{style="note"}

## Checkpoint ✅

Você agora domina:
- [x] Estrutura completa do arquivo
- [x] Configurações de serviço
- [x] Networking avançado
- [x] Gerenciamento de volumes
- [x] Configs e secrets
- [x] Deploy configurations
- [x] Exemplos práticos
- [x] Boas práticas

## Próximos Passos 🎯

1. [Docker Compose Commands](docker-compose-commands.md)
2. [Container Operations](container-operations.md)
3. [Best Practices](best-practices.md)

## Recursos Adicionais 📚

- [Compose File Reference](https://docs.docker.com/compose/compose-file/)
- [Compose Specification](https://github.com/compose-spec/compose-spec)
- [Docker Compose Examples](https://github.com/docker/awesome-compose)

> **Sensei's Note:** "O arquivo docker-compose.yml é como uma receita - cada ingrediente e sua medida importa para o resultado final! 🍜"
{style="note"}