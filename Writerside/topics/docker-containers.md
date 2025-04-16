# Docker Containers: O Guia Definitivo 🐳

```ascii
╔═══════════════════════════════════════════════════════════╗
║ DOCKER CONTAINERS GUIDE v1.0                              ║
║                                                          ║
║ STATUS: READY TO DEPLOY                                  ║
║ LEVEL: BEGINNER TO ADVANCED                              ║
║ MISSION: DOMINAR A ARTE DOS CONTAINERS                   ║
║                                                          ║
╚═══════════════════════════════════════════════════════════╝
```

## Conceitos Fundamentais 🎯

### O que é um Container?
Um container é uma unidade padrão de software que empacota código e todas as suas dependências para que o aplicativo seja executado de forma rápida e confiável em diferentes ambientes computacionais.

### Características Principais
- Isolamento de processos
- Compartilhamento do kernel do host
- Portabilidade entre ambientes
- Recursos controlados e limitados
- Inicialização rápida

## Comandos Essenciais 🛠️

### Gerenciamento Básico
```bash
# Criar e executar container
docker run -d nginx

# Listar containers ativos
docker ps

# Parar container
docker stop container_id

# Remover container
docker rm container_id
```

### Operações Avançadas
```bash
# Executar comando no container
docker exec -it container_id bash

# Inspecionar container
docker inspect container_id

# Ver logs
docker logs container_id

# Copiar arquivos
docker cp arquivo.txt container_id:/path/
```

## Configurações Importantes ⚙️

### Networking
```bash
# Expor porta
docker run -p 8080:80 nginx

# Conectar a rede
docker run --network minha-rede nginx

# DNS personalizado
docker run --dns 8.8.8.8 nginx
```

### Storage
```bash
# Montar volume
docker run -v /host/path:/container/path nginx

# Bind mount
docker run --mount type=bind,source=/host,target=/container nginx

# Volume temporário
docker run --tmpfs /app/temp nginx
```

## Boas Práticas 📋

### DO's ✅
- Use tags específicas
- Defina limites de recursos
- Implemente health checks
- Monitore seus containers
- Mantenha logs organizados

### DON'Ts ❌
- Não armazene dados sensíveis
- Evite containers muito grandes
- Não use a tag `latest`
- Não execute como root
- Não ignore security scans

## Troubleshooting Matrix 🔍

| Problema | Sintoma | Comando de Debug | Solução |
|----------|---------|-----------------|----------|
| Container não inicia | Exit code != 0 | `docker logs` | Verificar logs e configurações |
| Memória insuficiente | OOM Killer | `docker stats` | Aumentar limite de memória |
| Rede inacessível | Connection refused | `docker network inspect` | Verificar configurações de rede |
| Disco cheio | No space left | `docker system df` | Limpar recursos não utilizados |

## Monitoramento e Logs 📊

### Stats em Tempo Real
```bash
# Métricas básicas
docker stats

# Formato personalizado
docker stats --format "table {{.Container}}\t{{.CPUPerc}}\t{{.MemUsage}}"

# Logs com timestamp
docker logs --timestamps container_id
```

## Segurança e Compliance 🔒

### Hardening Básico
```bash
# Limitar recursos
docker run --memory 512m --cpus 2 nginx

# Usuário não-root
docker run --user 1000:1000 nginx

# Capabilities limitadas
docker run --cap-drop ALL nginx
```

## Automação e Scripts 🤖

### Exemplos Práticos
```bash
# Reiniciar containers com base em uso de memória
docker stats --no-stream --format "{{.Container}} {{.MemPerc}}" | \
while read container memory; do
  if [ "${memory%.*}" -gt 90 ]; then
    echo "Reiniciando $container (Memória: $memory)"
    docker restart $container
  fi
done
```

## Próximos Passos 🎮

1. Explore containers avançados
2. Aprenda sobre orquestração
3. Implemente monitoring
4. Estude práticas de segurança
5. Desenvolva pipelines CI/CD

> **Container-chan diz:** "Lembre-se, um container feliz é um container bem configurado! 🌸"
{style="tip"}

> **Docker-sama alerta:** "Sempre monitore seus containers em produção! 🚨"
{style="warning"}