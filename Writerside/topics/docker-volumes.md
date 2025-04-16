# Docker Volumes: Persistência e Performance 💾

```ascii
╔═══════════════════════════════════════╗
║           VOLUME ANATOMY              ║
║                                       ║
║    [Container] ←→ [Volume] ←→ [Host]  ║
║         ↑          ↑         ↑        ║
║    [Bind Mount] [tmpfs] [Block Device]║
║                                       ║
╚═══════════════════════════════════════╝
```

## Volume Types 📦

### Named Volumes
```bash
# Criar volume nomeado
docker volume create data-volume

# Usar volume
docker run -v data-volume:/app/data nginx

# Inspecionar volume
docker volume inspect data-volume
```

### Bind Mounts
```bash
# Montar diretório do host
docker run -v /host/path:/container/path nginx

# Modo somente leitura
docker run -v /host/config:/app/config:ro nginx
```

### tmpfs Mounts
```bash
# Montar volume em memória
docker run --tmpfs /app/cache nginx

# Configurar tamanho
docker run --tmpfs /app/cache:size=100M nginx
```

## Volume Management 🛠️

### Basic Operations
```bash
# Listar volumes
docker volume ls

# Remover volume
docker volume rm volume_name

# Remover volumes não utilizados
docker volume prune

# Backup de volume
docker run --rm \
  -v volume_name:/source \
  -v $(pwd):/backup \
  alpine tar -czf /backup/volume.tar.gz -C /source .
```

### Volume Drivers
```bash
# Criar volume com driver específico
docker volume create \
  --driver local \
  --opt type=btrfs \
  --opt device=/dev/sda1 \
  btrfs-volume

# Volume NFS
docker volume create \
  --driver local \
  --opt type=nfs \
  --opt o=addr=192.168.1.1,rw \
  --opt device=:/path/to/dir \
  nfs-volume
```

## Performance Optimization 🚀

### Cache Configuration
```bash
# Volume para cache
docker volume create \
  --driver local \
  --opt type=tmpfs \
  --opt device=tmpfs \
  --opt o=size=1g \
  cache-volume

# Usar cache
docker run \
  -v cache-volume:/app/cache \
  --memory-swappiness=0 \
  app
```

### I/O Tuning
```yaml
version: '3.8'
services:
  db:
    volumes:
      - data:/var/lib/mysql
    deploy:
      resources:
        limits:
          blkio-weight: 1000

volumes:
  data:
    driver_opts:
      type: 'none'
      o: 'bind'
      device: '/mnt/high-perf-disk'
```

## Docker Compose Integration 📋

### Volume Configuration
```yaml
version: '3.8'
services:
  web:
    volumes:
      - web-data:/var/www/html
      - ./config:/etc/nginx:ro
      - type: tmpfs
        target: /tmp
        tmpfs:
          size: 100M

volumes:
  web-data:
    driver: local
    driver_opts:
      type: 'none'
      o: 'bind'
      device: '/data/web'
```

### Backup Strategy
```yaml
services:
  backup:
    image: alpine
    volumes:
      - data-volume:/source:ro
      - ./backups:/backup
    command: tar -czf /backup/backup.tar.gz -C /source .
    deploy:
      restart_policy:
        condition: none
```

## Volume Security 🔒

### Permission Management
```bash
# Definir permissões
docker run \
  -v data:/app/data \
  --user 1000:1000 \
  app

# SELinux context
docker run \
  -v data:/app/data:z \
  app
```

### Access Control
```yaml
services:
  app:
    volumes:
      - type: volume
        source: sensitive-data
        target: /app/data
        read_only: true
        volume:
          nocopy: true
```

## Troubleshooting 🔍

### Common Issues
```bash
# Verificar montagens
docker inspect -f '{{ .Mounts }}' container_id

# Checar permissões
docker run --rm -v problem-volume:/vol alpine ls -la /vol

# Verificar espaço
docker system df -v
```

### Debug Tools
```bash
# Inspecionar volume
docker volume inspect volume_name

# Testar acesso
docker run --rm \
  -v volume_name:/test \
  busybox touch /test/testfile

# Verificar logs
docker logs container_id 2>&1 | grep volume
```

## Best Practices 💡

### Volume Design
1. Use named volumes para dados persistentes
2. Bind mounts para desenvolvimento
3. tmpfs para dados sensíveis temporários
4. Planeje backup e restore
5. Monitore uso de espaço

### Performance Tips
1. Use volumes locais para I/O intensivo
2. Configure cache apropriadamente
3. Evite bind mounts em produção
4. Monitore performance de I/O
5. Use volumes dedicados para dados críticos

## Waifu Volume Tips 🎀

> **Volume-chan diz:** "Sempre faça backup dos seus dados importantes! 💾"
{style="tip"}

> **Mount-sama alerta:** "Cuidado com permissões em bind mounts! ⚠️"
{style="warning"}

> **Cache-chan sugere:** "Use tmpfs para dados temporários sensíveis! 🔒"
{style="note"}

## Próximos Passos 🎯

1. [Container Storage](container-storage.md)
2. [Performance Tuning](performance-tuning.md)
3. [Backup Strategies](backup-strategies.md)

## Referências 📚

- [Docker Volumes Overview](https://docs.docker.com/storage/volumes/)
- [Volume Plugins](https://docs.docker.com/engine/extend/plugins_volume/)
- [Storage Best Practices](https://docs.docker.com/storage/best-practices/)
- [Volume Security Guide](https://docs.docker.com/storage/security/)

> **Volume Master's Note:** "Volumes são a memória persistente dos seus containers - trate-os com carinho! 🚀"
{style="note"}