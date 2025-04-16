# Container Logging: Dominando os Logs 📝

```ascii
╔═══════════════════════════════════════════════════════════╗
║ CONTAINER LOGGING MATRIX                                  ║
║                                                          ║
║ stdout/stderr → driver → aggregator → storage → analysis ║
║                                                          ║
║ STATUS: CAPTURING CONTAINER INSIGHTS...                  ║
╚═══════════════════════════════════════════════════════════╝
```

## Logging Basics 🔰

### Comandos Essenciais
```bash
# Logs básicos
docker logs container_id

# Follow logs em tempo real
docker logs -f container_id

# Logs com timestamp
docker logs -t container_id

# Últimas N linhas
docker logs --tail 100 container_id

# Logs desde timestamp
docker logs --since 2023-01-01T00:00:00 container_id
```

## Log Drivers 🚗

### Configuração de Driver
```json
{
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m",
    "max-file": "3"
  }
}
```

### Drivers Disponíveis
| Driver | Uso | Descrição |
|--------|-----|-----------|
| json-file | Default | Logs em formato JSON |
| local | Local | Logs persistentes locais |
| syslog | System | Logs para syslog |
| journald | SystemD | Logs para journald |
| fluentd | Aggregation | Logs para Fluentd |
| splunk | Enterprise | Logs para Splunk |

## Advanced Logging 🎓

### Fluentd Integration
```yaml
version: '3'
services:
  app:
    logging:
      driver: fluentd
      options:
        fluentd-address: localhost:24224
        tag: docker.{{.Name}}
```

### ELK Stack Setup
```yaml
version: '3'
services:
  filebeat:
    image: elastic/filebeat
    volumes:
      - /var/lib/docker/containers:/var/lib/docker/containers:ro
      - /var/run/docker.sock:/var/run/docker.sock:ro
```

## Logging Best Practices 🎯

### DO's ✅
- Configure log rotation
- Use estruturado logging
- Implemente log levels
- Centralize logs
- Monitore log size
- Backup logs importantes

### DON'Ts ❌
- Não ignore log rotation
- Evite logs verbosos demais
- Não armazene dados sensíveis
- Não negligencie log parsing
- Evite duplicação de logs

## Troubleshooting Matrix 🔍

| Problema | Sintoma | Solução |
|----------|---------|---------|
| Log cheio | Disk space | Configurar rotation |
| Log lento | Performance | Ajustar buffer size |
| Log perdido | Missing data | Verificar driver |
| Log duplicado | Redundancy | Checar configuração |

## Logging Patterns 📊

### Structured Logging
```json
{
  "level": "info",
  "timestamp": "2023-01-01T00:00:00Z",
  "service": "api",
  "message": "Request processed",
  "details": {
    "method": "GET",
    "path": "/api/v1/users",
    "duration": "125ms"
  }
}
```

### Log Aggregation
```ascii
┌──────────┐    ┌──────────┐    ┌──────────┐
│Container │───►│Collector │───►│  Store   │
└──────────┘    └──────────┘    └──────────┘
     │               │               │
     └───────────────┴───────────────┘
         Distributed Logging Flow
```

## Waifu Logging Tips 💡

> **Log-chan diz:** "Logs são como pegadas digitais - mantenha-os organizados! 📝"
{style="tip"}

> **Debug-sama alerta:** "Sempre configure log rotation, ou seu disco ficará triste! 💾"
{style="warning"}

> **Trace-chan sugere:** "Use log levels sabiamente para facilitar o debugging! 🔍"
{style="note"}

## Monitoring Integration 📊

### Prometheus Format
```yaml
logging:
  metrics:
    - name: log_messages_total
      type: counter
      labels:
        level: ${level}
        service: ${service}
```

### Grafana Dashboard
```json
{
  "panels": [
    {
      "title": "Log Volume",
      "type": "graph",
      "targets": [
        {
          "expr": "rate(log_messages_total[5m])",
          "legendFormat": "{{level}}"
        }
      ]
    }
  ]
}
```

## Quick Commands 🚀

```bash
# Debug logging
docker logs --debug container_id

# Export logs
docker logs container_id > app.log

# Multi-container logs
docker compose logs -f service_name

# Log stats
docker logs --details container_id
```

## Checkpoint ✅

Você agora domina:
- [x] Comandos básicos de logging
- [x] Log drivers
- [x] Log rotation
- [x] Structured logging
- [x] Log aggregation
- [x] Troubleshooting
- [x] Best practices

## Próximos Passos 🎯

1. [Container Monitoring](container-monitoring.md)
2. [Container Operations](container-operations.md)
3. [Troubleshooting Guide](troubleshooting-guide.md)

> **Master Logger's Note:** "Logs são seus olhos no mundo dos containers - mantenha-os limpos e afiados! 👀"
{style="note"}