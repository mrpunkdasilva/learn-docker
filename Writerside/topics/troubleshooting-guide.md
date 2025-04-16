# Guia de Troubleshooting: Debugando na Matrix 🔍

```ascii
╔═══════════════════════════════════════════════════════════╗
║ DOCKER DEBUG MATRIX v1.0                                  ║
║                                                          ║
║ STATUS: SCANNING FOR ISSUES...                           ║
║ LEVEL: FROM BASIC TO ADVANCED                            ║
║ MISSION: RESOLVER PROBLEMAS COMO UM PRO                  ║
║                                                          ║
╚═══════════════════════════════════════════════════════════╝
```

## Problemas Comuns e Soluções 🎯

### 1. Container não Inicia
```bash
# Verificar logs
docker logs container_id

# Verificar status
docker ps -a

# Inspecionar configuração
docker inspect container_id
```

### 2. Problemas de Rede
```bash
# Verificar redes
docker network ls

# Testar conectividade
docker run --rm busybox ping container_name

# Inspecionar rede
docker network inspect bridge
```

### 3. Problemas de Volume
```bash
# Listar volumes
docker volume ls

# Inspecionar volume
docker volume inspect volume_name

# Verificar permissões
ls -la /var/lib/docker/volumes/
```

## Ferramentas de Debug Avançadas 🛠️

### Docker Stats
```bash
# Monitorar recursos
docker stats

# Limitar recursos
docker run --memory=512m --cpu-shares=512 image_name
```

### Docker Events
```bash
# Monitorar eventos em tempo real
docker events

# Filtrar eventos
docker events --filter 'type=container'
```

### Logging Avançado
```bash
# Logs com timestamp
docker logs --timestamps container_id

# Logs em tempo real
docker logs --follow container_id

# Últimas N linhas
docker logs --tail 100 container_id
```

## Técnicas de Investigação 🔎

### 1. Container Deep Dive
```bash
# Acessar container
docker exec -it container_id sh

# Processos rodando
docker top container_id

# Histórico de comandos
docker history image_name
```

### 2. Network Debugging
```bash
# DNS lookup
docker run --rm busybox nslookup container_name

# Trace route
docker run --rm busybox traceroute host.docker.internal

# Port mapping
docker port container_id
```

### 3. Storage Analysis
```bash
# Disk usage
docker system df

# Clean up
docker system prune -a

# Volume cleanup
docker volume prune
```

## Waifu Debug Tips 💡

> **Debug-chan diz:** "Logs são como pegadas digitais - siga-as e encontrará a verdade! 🔍"
{style="tip"}

> **Network-sama alerta:** "Sempre verifique suas configurações de rede duas vezes! 🌐"
{style="warning"}

## Checklist de Debug ✅

### Básico
- [ ] Verificar logs do container
- [ ] Confirmar status do container
- [ ] Checar configurações de rede
- [ ] Validar volumes montados

### Avançado
- [ ] Analisar métricas de performance
- [ ] Investigar eventos do sistema
- [ ] Verificar segurança
- [ ] Validar recursos disponíveis

## Matriz de Decisão 📊

| Sintoma | Primeira Ação | Próxima Ação | Solução Comum |
|---------|---------------|--------------|---------------|
| Container Crashando | `docker logs` | `docker inspect` | Verificar recursos |
| Rede Lenta | `docker network inspect` | `ping test` | Ajustar DNS |
| Disco Cheio | `docker system df` | `docker system prune` | Limpar volumes |
| CPU Alta | `docker stats` | `top` no container | Limitar recursos |

## Comandos de Emergência 🚨

### Reset Rápido
```bash
# Parar todos containers
docker stop $(docker ps -q)

# Remover containers parados
docker container prune

# Limpar sistema
docker system prune -af
```

### Backup de Emergência
```bash
# Backup de volume
docker run --rm -v volume_name:/source -v $(pwd):/backup alpine tar -czf /backup/volume_backup.tar.gz -C /source .

# Backup de container
docker commit container_id backup_image
```

## Próximos Passos 🎯

1. [Laboratório Prático](hands-on-lab.md)
2. [Melhores Práticas](best-practices.md)
3. [Monitoramento Avançado](container-monitoring.md)

> **Sensei's Note:** "O melhor debug é aquele que você não precisa fazer - monitore proativamente!"
{style="note"}