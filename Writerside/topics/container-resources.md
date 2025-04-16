# Container Resources Management 📊

## Resource Limits Overview 🎯

### Basic Resource Types
```ascii
┌────────────────────┐
│ CPU                │
│ Memory             │
│ I/O                │
│ Network Bandwidth  │
└────────────────────┘
```

## CPU Management ⚡

### CPU Limits
```bash
# Limitar CPUs
docker run --cpus=2 nginx

# Definir CPU shares
docker run --cpu-shares=512 nginx

# Especificar CPUs específicas
docker run --cpuset-cpus="0,2" nginx
```

### CPU Quotas
```yaml
version: '3.8'
services:
  app:
    deploy:
      resources:
        limits:
          cpus: '0.50'
        reservations:
          cpus: '0.25'
```

## Memory Management 💾

### Memory Limits
```bash
# Limite de memória
docker run --memory=512m nginx

# Limite de swap
docker run --memory-swap=1g nginx

# Reserva de memória
docker run --memory-reservation=256m nginx
```

### Memory Settings Matrix
| Parâmetro | Descrição | Exemplo |
|-----------|-----------|---------|
| --memory | Limite máximo | --memory=1g |
| --memory-swap | Limite swap | --memory-swap=2g |
| --memory-reservation | Reserva soft | --memory-reservation=512m |
| --kernel-memory | Memória kernel | --kernel-memory=256m |

## Storage Management 💽

### Storage Options
```bash
# Limitar escrita
docker run --device-write-bps /dev/sda:1mb nginx

# Limitar leitura
docker run --device-read-bps /dev/sda:1mb nginx

# Limitar IOPS
docker run --device-write-iops /dev/sda:100 nginx
```

### Storage Quotas
```yaml
services:
  db:
    volumes:
      - data:/var/lib/mysql
    deploy:
      resources:
        limits:
          storage: 5G
```

## Network Management 🌐

### Network Controls
```bash
# Limitar banda
docker run --network-bandwidth=100mb nginx

# QoS settings
docker run --network-qos=high nginx
```

## Monitoring Resources 📈

### Resource Usage
```bash
# Stats em tempo real
docker stats

# Format específico
docker stats --format "table {{.Container}}\t{{.CPUPerc}}\t{{.MemUsage}}"

# Batch monitoring
docker stats --no-stream
```

### Resource Metrics
```bash
# Métricas detalhadas
docker container inspect container_id

# Logs de recursos
docker logs --details container_id
```

## Resource Optimization 🔧

### Best Practices
1. ✅ Defina limites realistas
2. ✅ Monitore uso constante
3. ✅ Use reservations
4. ✅ Configure alertas
5. ✅ Otimize regularmente

### Anti-Patterns
1. ❌ Sem limites definidos
2. ❌ Overcommit recursos
3. ❌ Ignorar métricas
4. ❌ Configuração genérica
5. ❌ Negligenciar monitoramento

## Resource Templates 📋

### Development Template
```yaml
resources:
  limits:
    cpu: 0.5
    memory: 512M
  reservations:
    cpu: 0.1
    memory: 128M
```

### Production Template
```yaml
resources:
  limits:
    cpu: 2
    memory: 2G
  reservations:
    cpu: 1
    memory: 1G
```

## Troubleshooting Guide 🔍

### Common Issues
1. **OOM Kills**
   ```bash
   # Verificar logs
   dmesg | grep -i kill
   
   # Aumentar limite
   docker update --memory 1g container_id
   ```

2. **CPU Throttling**
   ```bash
   # Verificar throttling
   docker stats --no-stream --format "{{.CPUPerc}}"
   
   # Ajustar limite
   docker update --cpus 2 container_id
   ```

## Waifu Resource Tips 💡

> **Resource-chan diz:** "Balancear recursos é como fazer um bento box perfeito - cada item precisa do espaço certo!"
{style="tip"}

> **Monitor-sama alerta:** "Sempre monitore seus recursos, ou prepare-se para o caos!"
{style="warning"}

## Quick Reference 📚

### Resource Commands
```bash
# CPU
--cpus=<value>
--cpu-shares=<value>
--cpuset-cpus=<value>

# Memory
--memory=<value>
--memory-swap=<value>
--memory-reservation=<value>

# Storage
--device-write-bps=<value>
--device-read-bps=<value>

# Network
--network-bandwidth=<value>
```

### Monitoring Commands
```bash
# Basic monitoring
docker stats

# Detailed info
docker inspect

# Resource update
docker update
```

## Next Steps 🎯

1. [Container Monitoring](container-monitoring.md)
2. [Performance Optimization](docker-performance.md)
3. [Troubleshooting Guide](troubleshooting-guide.md)

> **Resource Master's Note:** "Um container bem configurado é como um jardim zen - equilibrado e eficiente."
{style="note"}