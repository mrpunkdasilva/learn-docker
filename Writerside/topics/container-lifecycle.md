# Ciclo de Vida dos Containers: Do Nascimento à Transcendência 🔄

```ascii
╔═══════════════════════════════════════════════════════════╗
║ CONTAINER LIFECYCLE STATES                                ║
║                                                          ║
║    created ──► running ◄──► paused                       ║
║        │         │            │                          ║
║        │         └──► stopped ◄┘                         ║
║        │              │                                  ║
║        └──────────────► removed                         ║
║                                                          ║
╚═══════════════════════════════════════════════════════════╝
```

## Estados Fundamentais 🎭

### 1. Created
```bash
# Criar container sem iniciar
docker create --name meu-container nginx

# Verificar estado
docker ps -a | grep meu-container

# Criar com configurações específicas
docker create \
  --name web-app \
  --memory 512m \
  --cpus 2 \
  --network bridge \
  nginx:latest
```

### 2. Running
```bash
# Iniciar container
docker start meu-container

# Criar e iniciar imediatamente
docker run -d --name outro-container nginx

# Executar com limites e variáveis
docker run -d \
  --name app \
  -p 8080:80 \
  -v /data:/app/data \
  -e NODE_ENV=production \
  --restart unless-stopped \
  node:alpine
```

### 3. Paused
```bash
# Pausar container
docker pause meu-container

# Despausar container
docker unpause meu-container

# Pausar múltiplos containers
docker pause $(docker ps -q)
```

### 4. Stopped
```bash
# Parar container gracefully
docker stop meu-container

# Parar container imediatamente
docker kill meu-container

# Parar com timeout personalizado
docker stop --time 30 meu-container
```

### 5. Removed
```bash
# Remover container parado
docker rm meu-container

# Forçar remoção
docker rm -f meu-container

# Remover todos containers parados
docker container prune
```

## Transições de Estado Avançadas 🔄

### Restart Policies
```bash
# No - não reinicia automaticamente
docker run --restart no nginx

# Always - sempre reinicia
docker run --restart always nginx

# On-failure - reinicia em caso de erro
docker run --restart on-failure:3 nginx

# Unless-stopped - reinicia exceto se parado manualmente
docker run --restart unless-stopped nginx
```

### Gerenciamento de Recursos
```bash
# Limitar memória e CPU
docker run \
  --memory 512m \
  --memory-swap 1g \
  --cpus 2 \
  --cpu-shares 1024 \
  nginx

# Atualizar limites em runtime
docker update \
  --memory 1g \
  --cpus 4 \
  container_id
```

## Monitoramento e Logging 📊

### Health Checks
```bash
# Configurar health check
docker run -d \
  --name web \
  --health-cmd="curl -f http://localhost/ || exit 1" \
  --health-interval=5s \
  --health-retries=3 \
  --health-timeout=2s \
  nginx
```

### Logs
```bash
# Ver logs com timestamp
docker logs --timestamps container_id

# Follow logs em tempo real
docker logs -f container_id

# Logs desde específico timestamp
docker logs --since 2023-01-01T00:00:00 container_id
```

## Waifu Container Tips 💡

> **Lifecycle-chan diz:** "Containers são efêmeros! Trate-os como borboletas digitais! 🦋"
{style="tip"}

> **State-sama alerta:** "Sempre use stop antes de rm para um graceful shutdown! 🛑"
{style="warning"}

> **Debug-chan sugere:** "Logs são como pegadas digitais - siga-as para encontrar problemas! 🔍"
{style="tip"}

## Checkpoint Final ✅

Você agora domina:
- [x] Estados e transições dos containers
- [x] Políticas de restart
- [x] Monitoramento básico
- [x] Health checks
- [x] Logging
- [x] Gerenciamento de recursos

## Próximos Passos 🎯

1. [Container Operations](container-operations.md)
2. [Container Runtime](container-runtime.md)
3. [Container Monitoring](container-monitoring.md)

> **Sensei's Note:** "Dominar o ciclo de vida dos containers é como se tornar um mestre Jedi - requer paciência, prática e sabedoria."
{style="note"}
