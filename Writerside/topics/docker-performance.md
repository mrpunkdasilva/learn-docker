# Docker Performance Optimization 🚀

```ascii
╔════════════════════════════════════════════════════╗
║  DOCKER PERFORMANCE OPTIMIZATION                   ║
║                                                   ║
║  CPU | Memory | I/O | Network | Build | Runtime   ║
╚════════════════════════════════════════════════════╝
```

## Resource Management 📊

### CPU Optimization
```bash
# Limitar CPU
docker run --cpus=".5" nginx

# Definir CPU shares
docker run --cpu-shares=512 nginx

# Pinning em CPUs específicas
docker run --cpuset-cpus="0,1" nginx
```

### Memory Management
```bash
# Limitar memória
docker run --memory="512m" nginx

# Configurar swap
docker run --memory-swap="1g" nginx

# Reservar memória
docker run --memory-reservation="256m" nginx
```

## Storage Performance 💾

### Storage Driver Selection
```yaml
{
  "storage-driver": "overlay2",
  "storage-opts": [
    "overlay2.override_kernel_check=true"
  ]
}
```

### Volume Performance
```bash
# Volume local otimizado
docker volume create --driver local \
  --opt type=tmpfs \
  --opt device=tmpfs \
  --opt o=size=1g \
  fast-volume

# Monitorar I/O
docker stats --format "table {{.Name}}\t{{.BlockIO}}"
```

## Network Optimization 🌐

### Network Mode Selection
```bash
# Host networking
docker run --network host nginx

# Performance tuning
docker run \
  --network-alias=fastnet \
  --dns=8.8.8.8 \
  nginx
```

### Network Metrics
```bash
# Monitorar tráfego
docker stats --format "table {{.Name}}\t{{.NetIO}}"

# Análise detalhada
docker run --rm \
  --net container:target-container \
  nicolaka/netshoot iftop
```

## Build Performance ⚡

### Multi-stage Builds
```dockerfile
# Build stage
FROM golang:1.19 AS builder
WORKDIR /app
COPY . .
RUN CGO_ENABLED=0 go build -o app

# Production stage
FROM alpine:3.14
COPY --from=builder /app/app /
CMD ["/app"]
```

### Cache Optimization
```bash
# Usar buildkit
export DOCKER_BUILDKIT=1

# Cache em registry
docker buildx build \
  --cache-from type=registry,ref=user/app:cache \
  --cache-to type=registry,ref=user/app:cache \
  .
```

## Runtime Performance 🔄

### Container Stats
```bash
# Métricas em tempo real
docker stats --no-stream

# Formato personalizado
docker stats --format \
  "table {{.Container}}\t{{.CPUPerc}}\t{{.MemUsage}}\t{{.NetIO}}"
```

### Resource Quotas
```yaml
version: '3.8'
services:
  app:
    deploy:
      resources:
        limits:
          cpus: '0.50'
          memory: 512M
        reservations:
          cpus: '0.25'
          memory: 256M
```

## Monitoring Tools 📈

### Prometheus Integration
```yaml
metrics:
  prometheus:
    enabled: true
    port: 9323
```

### Grafana Dashboard
```bash
# Container metrics
rate(container_cpu_usage_seconds_total{container_name!=""}[5m])
sum(container_memory_usage_bytes) by (container_name)
```

## Performance Testing 🔍

### Load Testing
```bash
# Apache Benchmark
ab -n 1000 -c 10 http://localhost:8080/

# Hey HTTP load generator
hey -n 2000 -c 50 http://localhost:8080/
```

### Profiling
```bash
# CPU profile
docker run --cap-add=SYS_PTRACE \
  --security-opt seccomp=unconfined \
  app perf record -F 99 -p 1

# Memory profile
docker run -p 6060:6060 app
go tool pprof http://localhost:6060/debug/pprof/heap
```

## Best Practices Checklist ✅

### Container Level
1. [x] Resource limits configurados
2. [x] Logging driver otimizado
3. [x] Network mode apropriado
4. [x] Volume mounts eficientes
5. [x] Image size otimizado

### Host Level
1. [x] Storage driver adequado
2. [x] Sistema de arquivos otimizado
3. [x] Kernel parameters tuned
4. [x] Network stack otimizado
5. [x] Monitoramento ativo

## Performance Troubleshooting Matrix 🎯

| Sintoma | Métrica | Comando | Solução |
|---------|---------|---------|---------|
| CPU Alto | container_cpu_usage | `docker stats` | Ajustar limites |
| Memória Leak | container_memory_usage | `docker stats` | Heap analysis |
| I/O Lento | container_fs_io_current | `iostat` | Storage driver |
| Network Lag | container_network_transmit | `iftop` | Network mode |

## Advanced Tuning 🔧

### Kernel Parameters
```bash
# Ajustar sysctls
docker run --sysctl net.core.somaxconn=1024 nginx

# Configurar ulimits
docker run --ulimit nofile=65535:65535 nginx
```

### Runtime Flags
```bash
# Otimizar garbage collector
docker run -e "GOGC=100" app

# Tuning JVM
docker run -e "JAVA_OPTS=-XX:+UseG1GC" java-app
```

## Waifu Performance Tips 💫

> **Performance-chan diz:** "Monitore seus containers como se fossem seus tamagotchis!"
{style="tip"}

> **Resource-sama alerta:** "Limites são importantes, como porções de ramen!"
{style="warning"}

## Quick Reference 📚

### Common Commands
```bash
# CPU profile
docker stats --format "table {{.Container}}\t{{.CPUPerc}}"

# Memory analysis
docker stats --format "table {{.Container}}\t{{.MemUsage}}"

# I/O metrics
docker stats --format "table {{.Container}}\t{{.BlockIO}}"

# Network stats
docker stats --format "table {{.Container}}\t{{.NetIO}}"
```

### Performance Tools
1. cAdvisor
2. Prometheus
3. Grafana
4. netdata
5. ctop

> **Performance Master's Note:** "Performance é como um jardim zen - requer atenção constante e ajustes sutis."
{style="note"}