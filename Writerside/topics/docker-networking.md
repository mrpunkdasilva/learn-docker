# Docker Networking: O Guia Definitivo 🌐

```ascii
╔════════════════════════════════════════════╗
║           DOCKER NETWORK TOPOLOGY          ║
║                                           ║
║    [Container A] ←→ [Bridge] ←→ [Host]    ║
║         ↑             ↑          ↑        ║
║    [Container B] [Overlay] [MacVLAN]      ║
║                                           ║
╚════════════════════════════════════════════╝
```

## Network Drivers 🚗

### Bridge Network (Default)
```bash
# Criar rede bridge
docker network create --driver bridge minha-rede

# Conectar container
docker run -d --network minha-rede nginx

# Inspecionar rede
docker network inspect bridge
```

### Host Network
```bash
# Usar rede do host
docker run --network host nginx

# Verificar portas
netstat -tulpn
```

### None Network
```bash
# Sem rede
docker run --network none alpine
```

### Overlay Network
```bash
# Criar rede overlay
docker network create \
  --driver overlay \
  --attachable \
  --subnet=10.0.9.0/24 \
  overlay-net

# Habilitar criptografia
docker network create \
  --driver overlay \
  --opt encrypted \
  secure-overlay
```

### Macvlan
```bash
# Criar rede macvlan
docker network create \
  -d macvlan \
  --subnet=192.168.1.0/24 \
  --gateway=192.168.1.1 \
  -o parent=eth0 \
  macvlan-net
```

## Network Configuration 🔧

### Port Mapping
```bash
# Mapear porta única
docker run -p 8080:80 nginx

# Múltiplas portas
docker run \
  -p 80:80 \
  -p 443:443 \
  nginx

# UDP ports
docker run -p 53:53/udp dns-server

# Range de portas
docker run -p 8000-8010:8000-8010 app
```

### DNS Configuration
```bash
# Configurar DNS
docker run --dns 8.8.8.8 nginx

# Arquivo de configuração
cat /etc/docker/daemon.json
{
  "dns": ["8.8.8.8", "8.8.4.4"],
  "dns-search": ["example.com"]
}
```

## Network Security 🔒

### Network Policies
```yaml
# Network policy básica
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny
spec:
  podSelector: {}
  policyTypes:
  - Ingress
  - Egress
```

### Firewall Rules
```bash
# Regras iptables
iptables -I DOCKER-USER -i ext_if ! \
  -s 192.168.1.0/24 -j DROP

# Permitir portas específicas
iptables -A DOCKER-USER -p tcp \
  --dport 80 -j ACCEPT
```

## Network Troubleshooting 🔍

### Debug Commands
```bash
# Inspecionar rede
docker network inspect bridge

# Listar endpoints
docker network inspect -f \
  '{{range .Containers}}{{.Name}} {{.IPv4Address}}{{end}}' \
  bridge

# Estatísticas
docker stats --format \
  "table {{.Container}}\t{{.NetIO}}"
```

### Network Tools
```bash
# tcpdump
docker run --net=container:target \
  nicolaka/netshoot \
  tcpdump -i any port 80

# netstat
docker exec container netstat -tulpn

# ping test
docker run --rm busybox ping container_name
```

## Docker Compose Networking 📦

### Basic Configuration
```yaml
version: '3.8'
services:
  web:
    networks:
      - frontend
      - backend
  api:
    networks:
      backend:
        ipv4_address: 172.16.238.10

networks:
  frontend:
    driver: bridge
  backend:
    driver: bridge
    internal: true
```

### Advanced Features
```yaml
networks:
  overlay_net:
    driver: overlay
    driver_opts:
      encrypted: "true"
    ipam:
      driver: default
      config:
        - subnet: 10.0.9.0/24
```

## Network Monitoring 📊

### Basic Monitoring
```bash
# Network stats
docker stats --format \
  "table {{.Name}}\t{{.NetIO}}"

# Network events
docker events --filter 'type=network'

# Container connections
docker exec container ss -tulpn
```

### Advanced Monitoring
```bash
# Detailed metrics
docker run --rm \
  --net=container:target \
  nicolaka/netshoot \
  iftop

# Network profiling
docker run --rm \
  --net=container:target \
  nicolaka/netshoot \
  nethogs
```

## Best Practices 💡

### Network Design
1. Use user-defined networks
2. Isolate sensitive services
3. Implement network policies
4. Monitor network traffic
5. Regular security audits

### Performance Tips
1. Use host network when possible
2. Minimize network overhead
3. Optimize DNS resolution
4. Use appropriate drivers
5. Monitor bandwidth usage

## Waifu Network Tips 🎀

> **Network-chan diz:** "Sempre use redes customizadas para melhor isolamento! 🔒"
{style="tip"}

> **Bridge-sama alerta:** "Cuidado com conflitos de porta no host! ⚠️"
{style="warning"}

> **Overlay-chan sugere:** "Para multi-host, overlay networks são suas amigas! 🌐"
{style="note"}

## Próximos Passos 🎯

1. [Container Security](container-security.md)
2. [Docker Compose](docker-compose.md)
3. [Performance Tuning](performance-tuning.md)

## Referências 📚

- [Docker Networking Overview](https://docs.docker.com/network/)
- [Container Network Interface](https://github.com/containernetworking/cni)
- [Docker Network Drivers](https://docs.docker.com/network/drivers/)
- [Network Security Best Practices](https://docs.docker.com/network/security/)

> **Network Master's Note:** "Na rede dos containers, cada conexão é uma nova aventura! 🚀"
{style="note"}