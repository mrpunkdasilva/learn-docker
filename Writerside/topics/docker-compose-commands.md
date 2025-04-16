# Docker Compose Commands: Orquestrando com Maestria 🎼

```ascii
╔═══════════════════════════════════════════════════════════╗
║ DOCKER COMPOSE COMMAND REFERENCE                          ║
║                                                          ║
║ Up │ Down │ Build │ Run │ Exec │ Logs │ Config │ Scale   ║
║                                                          ║
║ Basic → Advanced → Debug → Production                     ║
╚═══════════════════════════════════════════════════════════╝
```

## Comandos Essenciais 🛠️

### Lifecycle Management
```bash
# Iniciar serviços
docker compose up              # Interativo
docker compose up -d          # Detached mode
docker compose up --build    # Força rebuild

# Parar serviços
docker compose down          # Para e remove
docker compose down -v      # Remove volumes também

# Gerenciamento
docker compose start        # Inicia serviços existentes
docker compose stop         # Para serviços
docker compose restart     # Reinicia serviços
```

### Build e Imagens
```bash
# Build
docker compose build         # Build todos serviços
docker compose build web    # Build serviço específico
docker compose pull        # Pull todas imagens

# Opções de Build
docker compose build --no-cache      # Ignora cache
docker compose build --pull         # Pull antes do build
```

## Monitoramento e Debug 🔍

### Logs e Status
```bash
# Logs
docker compose logs            # Todos os logs
docker compose logs -f        # Follow logs
docker compose logs web      # Logs de serviço específico

# Status
docker compose ps            # Lista containers
docker compose top          # Processos em execução
```

### Debug e Troubleshooting
```bash
# Execução
docker compose exec web sh   # Shell em container
docker compose run web bash # Novo container

# Inspeção
docker compose config       # Valida e mostra config
docker compose events      # Stream de eventos
```

## Comandos Avançados 🚀

### Scaling e Updates
```bash
# Escalar serviços
docker compose up -d --scale web=3

# Updates
docker compose pull         # Atualiza imagens
docker compose up --force-recreate
```

### Network e Volume
```bash
# Network
docker compose network ls
docker compose network prune

# Volumes
docker compose volume ls
docker compose volume prune
```

## Flags Importantes 🎯

| Flag | Descrição | Exemplo |
|------|-----------|---------|
| `-d` | Modo detached | `docker compose up -d` |
| `-f` | Arquivo específico | `docker compose -f prod.yml up` |
| `--profile` | Ativa profile | `docker compose --profile dev up` |
| `--env-file` | Arquivo .env | `docker compose --env-file .env.prod up` |

## One-Liners Poderosos 💪

```bash
# Restart rápido
docker compose down && docker compose up -d

# Limpar tudo
docker compose down -v --rmi all --remove-orphans

# Debug completo
docker compose logs -f --tail=100 web | grep ERROR

# Update com zero downtime
docker compose pull && docker compose up -d --no-deps web
```

## Waifu Compose Tips 🌸

> **Compose-chan diz:** "Use `docker compose config` para validar seu arquivo antes de executar! 📝"
{style="tip"}

> **Debug-chan alerta:** "Logs são seus melhores amigos na hora do troubleshooting! 🔍"
{style="warning"}

> **Scale-chan lembra:** "Cuidado ao escalar serviços com volumes nomeados! 💾"
{style="note"}

## Ambientes e Profiles 🌍

### Multi-ambiente
```bash
# Desenvolvimento
docker compose -f docker-compose.yml -f docker-compose.dev.yml up

# Produção
docker compose -f docker-compose.yml -f docker-compose.prod.yml up
```

### Profiles
```bash
# Ativar profile específico
docker compose --profile debug up

# Listar serviços por profile
docker compose --profile prod config --services
```

## Checkpoint ✅

Você agora domina:
- [x] Comandos básicos
- [x] Opções de build
- [x] Debug e logs
- [x] Scaling
- [x] Network e volumes
- [x] Multi-ambiente
- [x] Profiles

## Próximos Passos 🎯

1. [Docker Compose File](docker-compose-file.md)
2. [Container Operations](container-operations.md)
3. [Best Practices](best-practices.md)

## Quick Reference Card 📋

```ascii
╔════════════════════════════════════════════════════╗
║ DOCKER COMPOSE QUICK REFERENCE                     ║
╠════════════════════════════════════════════════════╣
║ up     │ Criar e iniciar serviços                 ║
║ down   │ Parar e remover serviços                 ║
║ build  │ Construir imagens                        ║
║ ps     │ Listar containers                        ║
║ logs   │ Ver logs                                 ║
║ exec   │ Executar comando                         ║
║ config │ Validar configuração                     ║
╚════════════════════════════════════════════════════╝
```

> **Sensei's Note:** "Docker Compose é como uma orquestra - cada comando é um instrumento que precisa estar em harmonia! 🎵"
{style="note"}