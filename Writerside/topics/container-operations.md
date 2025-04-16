# Operações com Containers: O Manual do Operador 🎮

```ascii
╔═══════════════════════════════════════════════════════════╗
║ CONTAINER LIFECYCLE                                       ║
║                                                          ║
║    created ──► running ◄──► paused                       ║
║        │         │            │                          ║
║        │         └──► stopped ◄┘                         ║
║        │              │                                  ║
║        └──────────────► removed                         ║
║                                                          ║
╚═══════════════════════════════════════════════════════════╝
```

## Operações Básicas 🛠️

### Ciclo de Vida
```bash
# Criar container
docker create --name meu-app nginx

# Iniciar container
docker start meu-app

# Parar container
docker stop meu-app

# Remover container
docker rm meu-app
```

### Execução e Acesso
```bash
# Executar novo container
docker run -d --name web nginx

# Executar comando em container
docker exec -it web bash

# Visualizar logs
docker logs web

# Copiar arquivos
docker cp arquivo.txt web:/app/
```

## Gerenciamento de Estado 📊

### Status e Inspeção
```bash
# Listar containers
docker ps -a

# Inspecionar container
docker inspect web

# Estatísticas em tempo real
docker stats web

# Processos em execução
docker top web
```

### Controle de Recursos
```bash
# Limitar CPU e memória
docker run -d \
  --cpus=".5" \
  --memory="512m" \
  --name app \
  nginx

# Atualizar limites
docker update --cpus=".8" app
```

## Network Operations 🌐

### Conectividade
```bash
# Criar rede
docker network create minha-rede

# Conectar container
docker network connect minha-rede web

# Desconectar container
docker network disconnect minha-rede web

# Inspecionar rede
docker network inspect minha-rede
```

## Volume Operations 💾

### Persistência de Dados
```bash
# Criar volume
docker volume create meus-dados

# Montar volume
docker run -d \
  -v meus-dados:/data \
  --name db \
  postgres

# Listar volumes
docker volume ls

# Remover volume
docker volume rm meus-dados
```

## Troubleshooting Matrix 🔍

| Problema | Comando | Solução |
|----------|---------|---------|
| Container não inicia | `docker logs` | Verificar logs de erro |
| Container travado | `docker restart` | Reiniciar container |
| Memória alta | `docker stats` | Ajustar limites |
| Rede instável | `docker network inspect` | Verificar conectividade |

## Batch Operations 🚀

### Operações em Massa
```bash
# Parar todos containers
docker stop $(docker ps -q)

# Remover containers parados
docker container prune

# Remover tudo não usado
docker system prune -a

# Backup de volumes
docker run --rm \
  -v volume_name:/source \
  -v $(pwd):/backup \
  alpine tar czf /backup/volume_backup.tar.gz /source
```

## Health Checks 💓

### Monitoramento de Saúde
```bash
# Container com health check
docker run -d \
  --health-cmd="curl -f http://localhost/ || exit 1" \
  --health-interval=5s \
  --name web \
  nginx

# Verificar status
docker inspect --format='{{.State.Health.Status}}' web
```

## Waifu Operation Tips 💡

> **Container-chan diz:** "Mantenha seus containers leves e eficientes! 🎈"
{style="tip"}

> **Resource-sama avisa:** "Sempre defina limites de recursos para evitar surpresas! ⚡"
{style="warning"}

## Debug Commands 🔧

### Ferramentas de Debug
```bash
# Logs em tempo real
docker logs -f web

# Histórico de eventos
docker events --filter container=web

# Dump de estado
docker inspect web > debug.json

# Trace de rede
docker run --net container:web nicolaka/netshoot
```

## Automation Scripts 🤖

### Exemplos de Automação
```bash
#!/bin/bash
# Restart containers mais velhos que 7 dias
docker ps -q | xargs docker inspect \
  --format='{{.Id}} {{.State.StartedAt}}' | \
  awk '{if ($2 <= "'$(date -d'7 days ago' -Ins --utc)'") print $1}' | \
  xargs docker restart
```

## Checkpoint ✅

Você agora sabe:
- [x] Gerenciar ciclo de vida
- [x] Controlar recursos
- [x] Operar redes
- [x] Gerenciar volumes
- [x] Debugar problemas
- [x] Automatizar operações

## Próximos Passos 🎯

1. [Melhores Práticas](best-practices.md)
2. [Laboratório Prático](hands-on-lab.md)
3. [Guia de Troubleshooting](troubleshooting-guide.md)

> **Sensei's Note:** "A maestria em operações de containers vem da prática constante e da atenção aos detalhes."
{style="note"}