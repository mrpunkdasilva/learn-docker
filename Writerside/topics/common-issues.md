# Problemas Comuns: Debugando na Matrix 🐛

```ascii
╔═══════════════════════════════════════════════════════════════════════╗
║  DOCKER TROUBLE MATRIX                                                ║
║                                                                       ║
║  [ERROR] - Permission denied                                          ║
║  [ERROR] - Port already in use                                        ║
║  [ERROR] - No space left on device                                    ║
║  [ERROR] - Cannot connect to the Docker daemon                        ║
║                                                                       ║
╚═══════════════════════════════════════════════════════════════════════╝
```

## Problemas de Permissão 🔒

### Sintoma
```bash
Got permission denied while trying to connect to the Docker daemon socket
```

### Solução
```bash
# Adicione seu usuário ao grupo docker
sudo usermod -aG docker $USER

# Recarregue os grupos (ou faça logout/login)
newgrp docker
```

## Conflito de Portas 🔌

### Sintoma
```bash
Error: Ports are not available: listen tcp 0.0.0.0:3000: bind: address already in use
```

### Solução
```bash
# Encontre o processo usando a porta
sudo lsof -i :3000

# Mate o processo (substitua PID)
sudo kill -9 PID

# OU use uma porta diferente
docker run -p 3001:3000 minha-app
```

## Sem Espaço em Disco 💾

### Sintoma
```bash
no space left on device
```

### Solução
```bash
# Limpe recursos não utilizados
docker system prune -a --volumes

# Verifique o uso de espaço
docker system df
```

## Daemon Offline 👻

### Sintoma
```bash
Cannot connect to the Docker daemon at unix:///var/run/docker.sock
```

### Solução
```bash
# Verifique o status do serviço
sudo systemctl status docker

# Reinicie o daemon
sudo systemctl restart docker
```

## Waifu Tips 💡

> **Debug-chan diz:** "Quando tudo falhar, tente `docker system prune`. Mas cuidado, isso é tipo formatar o PC - só que para containers!"
> {style="note"}

## Problemas de Rede 🌐

### Sintoma
```bash
ERROR: Network gateway has conflicts with existing routes
```

### Solução
```bash
# Recrie a rede default
docker network prune
docker network create bridge
```

## Problemas de Build 🏗️

### Sintoma
```bash
Step 3/8 : RUN npm install
npm ERR! code ECONNREFUSED
```

### Solução
```bash
# Verifique sua conexão
ping 8.8.8.8

# Use mirror alternativo
npm config set registry https://registry.npmmirror.com
```

## Checklist de Debug ✅

1. Verifique logs
   ```bash
   docker logs <container-id>
   ```

2. Inspecione o container
   ```bash
   docker inspect <container-id>
   ```

3. Entre no container
   ```bash
   docker exec -it <container-id> sh
   ```

4. Monitore recursos
   ```bash
   docker stats
   ```

## Prevenção é o Melhor Remédio 💊

### Boas Práticas
- Use `.dockerignore` apropriadamente
- Mantenha imagens base atualizadas
- Implemente health checks
- Monitore uso de recursos
- Faça backup de volumes importantes

### Ferramentas Úteis
- Portainer (GUI para gestão)
- ctop (top para containers)
- lazydocker (TUI amigável)
- docker-compose (orquestração local)

## Modo Hardcore: Debugging Avançado 🔍

### Análise de Sistema
```bash
# Verifique limites do sistema
ulimit -a

# Monitore eventos do Docker
docker events

# Debug de rede
docker network inspect bridge
```

### Logs Avançados
```bash
# Logs em tempo real com timestamp
docker logs -f --timestamps <container-id>

# Últimas 100 linhas
docker logs --tail 100 <container-id>
```

## Recursos de Emergência 🚨

- [Docker Debug Guide](https://docs.docker.com/engine/troubleshoot/)
- [Stack Overflow Docker Tag](https://stackoverflow.com/questions/tagged/docker)
- [Docker Forum](https://forums.docker.com/)

---

> "Todo bug é uma feature que ainda não encontrou seu propósito." - Hackerman