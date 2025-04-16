# CLI Basics: Dominando o Terminal Docker 🎮

```ascii
╔═══════════════════════════════════════════════════════════╗
║ DOCKER CLI COMMAND STRUCTURE                              ║
║                                                          ║
║ docker [options] command [arguments]                      ║
║                                                          ║
║ Examples:                                                ║
║ docker run nginx                                         ║
║ docker container ls                                      ║
║ docker image build .                                     ║
╚═══════════════════════════════════════════════════════════╝
```

## Comandos Essenciais 🛠️

### Sistema e Informações
```bash
# Verificar versão
docker version

# Informações do sistema
docker info

# Status do daemon
docker system df

# Limpar recursos não utilizados
docker system prune -a
```

### Gerenciamento de Containers
```bash
# Listar containers
docker ps          # Ativos
docker ps -a       # Todos
docker ps -q       # Apenas IDs
docker ps --size   # Com tamanho

# Criar e executar
docker run nginx
docker run -d --name web nginx    # Detached mode
docker run -it ubuntu bash        # Interativo

# Operações básicas
docker start web
docker stop web
docker restart web
docker rm web
```

### Gerenciamento de Imagens
```bash
# Listar imagens
docker images
docker image ls

# Baixar imagem
docker pull nginx:latest

# Remover imagem
docker rmi nginx
docker image rm nginx

# Construir imagem
docker build -t app:1.0 .
```

## Flags Comuns 🚩

### Run Flags
```bash
# Portas
-p 8080:80        # Host:Container

# Volumes
-v /host:/container

# Variáveis de ambiente
-e MYSQL_ROOT_PASSWORD=secret

# Recursos
--memory 512m
--cpus 2

# Network
--network bridge
```

### Formato de Saída
```bash
# Formatação JSON
docker ps --format '{{json .}}'

# Tabela customizada
docker ps --format 'table {{.ID}}\t{{.Names}}\t{{.Status}}'
```

## Network Commands 🌐

```bash
# Listar redes
docker network ls

# Criar rede
docker network create mynet

# Conectar container
docker network connect mynet container1

# Inspecionar rede
docker network inspect mynet
```

## Volume Commands 💾

```bash
# Listar volumes
docker volume ls

# Criar volume
docker volume create mydata

# Inspecionar volume
docker volume inspect mydata

# Remover volume
docker volume rm mydata
```

## Logs e Debug 🔍

```bash
# Ver logs
docker logs container1
docker logs -f container1    # Follow
docker logs --tail 100 container1

# Executar comando
docker exec -it container1 bash

# Inspecionar container
docker inspect container1
```

## Dicas de Produtividade ⚡

### Aliases Úteis
```bash
# Adicione ao seu .bashrc ou .zshrc
alias d='docker'
alias dc='docker-compose'
alias dps='docker ps'
alias dim='docker images'
```

### One-Liners Poderosos
```bash
# Parar todos containers
docker stop $(docker ps -q)

# Remover containers parados
docker container prune

# Remover imagens não utilizadas
docker image prune -a

# Backup de container
docker commit container1 backup/container1
```

## Waifu CLI Tips 💡

> **CLI-chan diz:** "Sempre use `--help` quando precisar! É como ter um manual de bolso! 📖"
{style="tip"}

> **Terminal-sama alerta:** "Cuidado com `docker system prune -a`! Ele é tipo um delete sem recovery! 🗑️"
{style="warning"}

## Troubleshooting Common Issues 🔧

| Erro | Causa Provável | Solução |
|------|----------------|---------|
| `Cannot connect to Docker daemon` | Docker não está rodando | `sudo systemctl start docker` |
| `Port is already allocated` | Porta em uso | Use outra porta ou libere a atual |
| `No space left on device` | Disco cheio | `docker system prune` |
| `Permission denied` | Não está no grupo docker | `sudo usermod -aG docker $USER` |

## Checkpoint ✅

Você agora sabe:
- [x] Estrutura básica dos comandos
- [x] Gerenciar containers
- [x] Gerenciar imagens
- [x] Trabalhar com redes
- [x] Gerenciar volumes
- [x] Debug e logs
- [x] Aliases e produtividade

## Quick Reference Card 📋

```ascii
╔════════════════════════════════════════════════════╗
║ DOCKER CLI CHEAT SHEET                            ║
╠════════════════════════════════════════════════════╣
║ docker run     → Criar/Executar container         ║
║ docker ps      → Listar containers                ║
║ docker images  → Listar imagens                   ║
║ docker build   → Construir imagem                 ║
║ docker exec    → Executar comando                 ║
║ docker logs    → Ver logs                         ║
║ docker inspect → Inspecionar objeto               ║
║ docker prune   → Limpar recursos                  ║
╚════════════════════════════════════════════════════╝
```

## Próximos Passos 🎯

1. [Docker Commands](docker-commands.md)
2. [Docker Images](docker-images.md)
3. [Docker Containers](docker-containers.md)
4. [Docker Compose](docker-compose-intro.md)
5. [Docker Networks](docker-networking.md)

> **Sensei's Note:** "O CLI é sua espada no mundo Docker. Mantenha-a afiada e saiba quando usá-la!"
{style="note"}