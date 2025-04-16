# Container Troubleshooting: A Arte do Debug 🔍

```ascii
╔═══════════════════════════════════════════════════════════╗
║ CONTAINER TROUBLESHOOTING MATRIX v2.0                     ║
║                                                          ║
║ [Container] → [Runtime] → [Network] → [Storage] → [Logs] ║
║                                                          ║
║ STATUS: DEBUGGING IN PROGRESS...                         ║
╚═══════════════════════════════════════════════════════════╝
```

## Quick Debug Commands 🚀

### Container Status
```bash
# Status geral
docker ps -a

# Detalhes do container
docker inspect container_id

# Métricas em tempo real
docker stats container_id

# Processos rodando
docker top container_id
```

### Logs e Eventos
```bash
# Logs com timestamp
docker logs -t container_id

# Eventos do sistema
docker events --filter container=container_id

# Logs em tempo real
docker logs -f container_id

# Debug mode
docker --debug run nginx
```

## Matriz de Problemas Comuns 📊

### 1. Container não Inicia
| Sintoma | Comando | Solução |
|---------|---------|---------|
| Exit Code != 0 | `docker logs` | Verificar logs de erro |
| OOM Killer | `docker stats` | Aumentar limite de memória |
| Port Conflict | `docker port` | Mudar mapeamento de porta |
| Missing Volume | `docker inspect` | Verificar volume mounts |

### 2. Performance Issues
| Sintoma | Comando | Solução |
|---------|---------|---------|
| CPU High | `docker stats` | Verificar processos |
| Memory Leak | `docker stats` | Analisar heap dump |
| I/O Slow | `iostat` | Verificar disco |
| Network Lag | `docker network` | Debug conectividade |

## Network Debugging 🌐

### Comandos de Rede
```bash
# DNS lookup
docker run --rm busybox nslookup container_name

# Network inspect
docker network inspect network_name

# Port mapping
docker port container_id

# Network stats
docker stats --format "table {{.Name}}\t{{.NetIO}}"
```

### Common Network Issues
1. DNS Resolution
   ```bash
   # Test DNS
   docker run --rm busybox ping 8.8.8.8
   ```

2. Port Conflicts
   ```bash
   # Check ports
   netstat -tulpn
   ```

## Storage Troubleshooting 💾

### Volume Debug
```bash
# List volumes
docker volume ls

# Inspect volume
docker volume inspect volume_name

# Check disk usage
docker system df -v
```

### Storage Issues Matrix
| Problema | Comando | Solução |
|----------|---------|---------|
| No Space | `docker system df` | Cleanup unused |
| Volume Missing | `docker volume ls` | Recreate volume |
| Permission Error | `ls -la /var/lib/docker` | Fix permissions |
| Mount Failed | `docker inspect` | Check mount points |

## Resource Management 📊

### Memory Issues
```bash
# Memory stats
docker stats --format "table {{.Name}}\t{{.MemUsage}}"

# Memory limits
docker update --memory 512m container_id

# OOM analysis
dmesg | grep -i kill
```

### CPU Debug
```bash
# CPU stats
docker stats --format "table {{.Name}}\t{{.CPUPerc}}"

# CPU limits
docker update --cpus 2 container_id
```

## Advanced Debug Techniques 🔬

### Container Deep Dive
```bash
# Shell access
docker exec -it container_id sh

# Process list
ps aux

# Network status
netstat -tulpn

# File system
df -h
```

### System Analysis
```bash
# System events
docker system events

# Resource usage
docker system df

# Detailed info
docker system info
```

## Debug Toolkit 🛠️

### Essential Tools
```bash
# Network tools
docker run --rm nicolaka/netshoot

# System analysis
docker run --rm -v /:/host alpine sh

# Performance analysis
docker run --rm --pid=host netflix/vector
```

## Waifu Debug Tips 💡

> **Debug-chan diz:** "Sempre comece pelos logs - eles contam a história completa! 📝"
{style="tip"}

> **Resource-sama alerta:** "Monitore seus recursos, ou enfrente a ira do OOM Killer! ⚡"
{style="warning"}

> **Network-chan sugere:** "Problemas de rede? Verifique DNS primeiro! 🌐"
{style="note"}

## Emergency Procedures 🚨

### Quick Recovery
```bash
# Stop all containers
docker stop $(docker ps -q)

# Remove stopped
docker container prune

# Clean system
docker system prune -af
```

### Backup Critical Data
```bash
# Volume backup
docker run --rm -v volume_name:/data -v $(pwd):/backup alpine tar -czf /backup/volume.tar.gz /data

# Container commit
docker commit container_id backup_image
```

## Debug Checklist ✅

### Basic Checks
- [ ] Verificar logs do container
- [ ] Confirmar status do container
- [ ] Checar uso de recursos
- [ ] Validar configuração de rede
- [ ] Verificar volumes montados

### Advanced Checks
- [ ] Analisar eventos do sistema
- [ ] Verificar logs do daemon
- [ ] Validar permissões
- [ ] Checar limites de recursos
- [ ] Investigar dependências

## Próximos Passos 🎯

1. [Container Monitoring](container-monitoring.md)
2. [Container Logging](container-logging.md)
3. [Container Security](container-security.md)

> **Debug Master's Note:** "Na matrix dos containers, cada erro é uma oportunidade de aprendizado! 🎓"
{style="note"}