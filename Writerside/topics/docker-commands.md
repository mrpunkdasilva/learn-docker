# Docker Commands: O Arsenal Completo 🚀

```ascii
╔═══════════════════════════════════════════════════════════╗
║ DOCKER COMMAND CATEGORIES                                 ║
║                                                          ║
║ Container │ Image │ Network │ Volume │ System │ Compose  ║
╚═══════════════════════════════════════════════════════════╝
```

## Container Commands 📦

### Lifecycle Management
```bash
# Criar e Executar
docker run [options] IMAGE
  -d, --detach          # Modo background
  -it                   # Modo interativo
  --name string         # Nome do container
  --rm                  # Remove após parar
  -p, --publish        # Mapear portas
  -v, --volume         # Montar volume

# Gerenciamento
docker start CONTAINER   # Iniciar
docker stop CONTAINER    # Parar
docker restart CONTAINER # Reiniciar
docker pause CONTAINER   # Pausar
docker unpause CONTAINER # Despausar
docker rm CONTAINER     # Remover
```

### Monitoramento e Debug
```bash
# Status e Logs
docker ps [options]      # Listar containers
docker logs CONTAINER    # Ver logs
docker stats CONTAINER   # Métricas em tempo real
docker top CONTAINER     # Processos
docker inspect CONTAINER # Info detalhada

# Execução
docker exec [options] CONTAINER COMMAND
docker attach CONTAINER  # Conectar ao container
docker cp SRC_PATH CONTAINER:DEST_PATH
```

## Image Commands 🖼️

### Gerenciamento de Imagens
```bash
# Básico
docker images         # Listar imagens
docker pull IMAGE     # Baixar imagem
docker push IMAGE     # Enviar imagem
docker rmi IMAGE      # Remover imagem

# Build
docker build [options] PATH
  -t, --tag         # Nomear imagem
  -f, --file        # Dockerfile alternativo
  --no-cache        # Ignorar cache

# Análise
docker history IMAGE # Ver camadas
docker inspect IMAGE # Info detalhada
docker save IMAGE    # Exportar
docker load         # Importar
```

## Network Commands 🌐

### Networking
```bash
# Gerenciamento
docker network create  # Criar rede
docker network ls     # Listar redes
docker network rm     # Remover rede
docker network prune  # Limpar não usadas

# Conexões
docker network connect    # Conectar container
docker network disconnect # Desconectar
docker network inspect   # Inspecionar
```

## Volume Commands 💾

### Storage
```bash
# Volumes
docker volume create  # Criar volume
docker volume ls     # Listar volumes
docker volume rm     # Remover volume
docker volume prune  # Limpar não usados

# Inspeção
docker volume inspect # Info detalhada
```

## System Commands ⚙️

### Manutenção
```bash
# Sistema
docker info        # Info do sistema
docker version     # Versão
docker events      # Eventos em tempo real

# Limpeza
docker system prune     # Limpar tudo
docker system df       # Uso de disco
docker system info     # Info do sistema
```

## Compose Commands 🎼

### Orquestração
```bash
# Básico
docker-compose up    # Criar e iniciar
docker-compose down  # Parar e remover
docker-compose ps    # Listar serviços

# Lifecycle
docker-compose start  # Iniciar serviços
docker-compose stop   # Parar serviços
docker-compose restart # Reiniciar
```

## Command Flags Matrix 🎯

| Flag | Uso | Exemplo |
|------|-----|---------|
| `-d` | Background | `docker run -d nginx` |
| `-p` | Portas | `docker run -p 80:80 nginx` |
| `-v` | Volumes | `docker run -v /host:/container nginx` |
| `-e` | Env vars | `docker run -e VAR=value nginx` |
| `--rm` | Auto-remove | `docker run --rm alpine` |

## One-Liners Poderosos 💪

```bash
# Parar todos containers
docker stop $(docker ps -aq)

# Remover tudo
docker system prune -af --volumes

# Backup de container
docker commit container backup/container:v1

# Container efêmero
docker run --rm -it alpine sh
```

## Waifu Command Tips 🌸

> **Command-chan diz:** "Use `--help` com qualquer comando para ver todas as opções! 📚"
{style="tip"}

> **Docker-sama alerta:** "Cuidado com comandos destrutivos como `prune` e `rm -f`! 💥"
{style="warning"}

## Debug Arsenal 🔍

### Troubleshooting Commands
```bash
# Logs em tempo real
docker logs -f container

# Métricas live
docker stats container

# Processos do container
docker top container

# Dump completo
docker inspect container > debug.json
```

## Quick Reference Card 📋

```ascii
╔════════════════════════════════════════════════════╗
║ DOCKER COMMANDS CHEAT SHEET                       ║
╠════════════════════════════════════════════════════╣
║ Container │ run, ps, start, stop, rm              ║
║ Image    │ build, pull, push, rmi                ║
║ Network  │ network create, connect, inspect       ║
║ Volume   │ volume create, ls, rm                 ║
║ System   │ info, version, prune                  ║
║ Compose  │ up, down, ps                          ║
╚════════════════════════════════════════════════════╝
```

## Checkpoint ✅

Você agora domina:
- [x] Comandos de container
- [x] Gerenciamento de imagens
- [x] Networking
- [x] Volumes
- [x] Manutenção do sistema
- [x] Docker Compose basics
- [x] Troubleshooting

## Próximos Passos 🎯

1. [Container Operations](container-operations.md)
2. [Advanced Networking](docker-networking.md)
3. [Volume Management](docker-volumes.md)
4. [System Administration](docker-admin.md)
5. [Compose Deep Dive](docker-compose-advanced.md)

> **Sensei's Note:** "Um verdadeiro mestre Docker conhece seus comandos como um samurai conhece sua espada!"
{style="note"}