# Container Networking

> "Em um mundo onde bits dançam através de cabos etéreos e pacotes viajam na velocidade da luz, eis que surge a mais intrigante das artes: a networking de containers." - Manifesto de um SRE Desiludido

## A Anatomia do Caos Digital 🧬

Ah, caro leitor! Permita-me guiá-lo através deste labirinto digital onde containers, qual habitantes de uma metrópole cyberpunk, comunicam-se através de pontes invisíveis e túneis etéreos. 

### O Teatro das Sombras Digitais
```ascii
         ╔══════════════════════════════╗
         ║    A MATRIX DOS CONTAINERS   ║
         ╚══════════════════════════════╝
              ↡                 ↡
    [Container A] ←→ [Container B]
         ↑    ╱╲              ↑
         │   ╱  ╲             │
         │  ╱    ╲            │
    [Network Namespace]  [Network Bridge]
              ↓
         [Host Network]
     "Onde os pacotes vão para morrer"
```

Imagine, se puder, um teatro onde cada container é um ator mascarado, interpretando seu papel em um palco virtual. Cada um possui sua própria identidade (IP), seu próprio camarim (namespace), e suas próprias linhas de comunicação (veth pairs).

### Os Fantasmas da Máquina
- **Network Namespaces**: Como apartamentos em um arranha-céu cyberpunk, cada container possui seu próprio espaço isolado. "Meu IP, minhas regras!"
- **Bridges Virtuais**: Pontes suspensas no vazio digital, conectando realidades paralelas. "A ponte para lugar nenhum, e ao mesmo tempo, para todo lugar."
- **veth pairs**: Cordões umbilicais digitais, nutrido bits e bytes. "Unidos na vida, unidos na morte."

## Os Drivers: Pilotos do Submundo Digital 🏎️

### Bridge Network: O Submundo Comum
Como becos escuros de uma cidade neon, a bridge network é onde a maioria dos containers vive - não é bonito, mas é funcional.

```ascii
╔═══════════════════════════════════╗
║  BRIDGE NETWORK: O GUETO DIGITAL  ║
╠═══════════════════════════════════╣
║ ┌──────┐  ┌──────┐  ┌──────┐     ║
║ │ Bot  │  │ Bot  │  │ Bot  │     ║
║ │ #127 │  │ #254 │  │ #666 │     ║
║ └──┬───┘  └──┬───┘  └──┬───┘     ║
║    └────────┬────────┬─┘         ║
║        [Bridge-01]                ║
╚═══════════════════════════════════╝
```

### Host Network: A Liberdade Perigosa
Ah, a rede do host! Como um mergulho sem paraquedas em Neo-Tokyo. Performance máxima, segurança mínima. Viva perigosamente!

```bash
# Para os corajosos (ou loucos)
docker run --network host nginx
# "Quem precisa de isolamento quando se tem velocidade?" - DevOps antes do incidente
```

### Overlay Network: A Teia da Ilusão
Multi-host networking, ou como gosto de chamar: "A arte de fazer containers acreditarem que estão na mesma rede quando, na verdade, estão separados por um abismo digital."

#### Configuração Multi-host
```bash
# Inicializar swarm
docker swarm init

# Criar rede overlay
docker network create \
  --driver overlay \
  --attachable \
  --subnet=10.0.9.0/24 \
  --gateway=10.0.9.1 \
  overlay_net

# Criptografia
docker network create \
  --driver overlay \
  --opt encrypted=true \
  secure_overlay
```

### Macvlan
- Interfaces MAC dedicadas
- Performance próxima ao bare metal
- Integração com redes físicas

#### Modos de Operação
```bash
# Modo Bridge
docker network create -d macvlan \
  --subnet=192.168.32.0/24 \
  --gateway=192.168.32.1 \
  -o parent=eth0 \
  -o macvlan_mode=bridge \
  macvlan_bridge

# Modo 802.1q trunk
docker network create -d macvlan \
  --subnet=192.168.32.0/24 \
  --gateway=192.168.32.1 \
  -o parent=eth0.10 \
  macvlan_trunk
```

## Container Network Interface (CNI)

### Arquitetura CNI
```ascii
┌────────────────────────────────────┐
│ CNI Pipeline                       │
├────────────────────────────────────┤
│ ADD → CHECK → DEL                  │
└────────────────────────────────────┘
```

### Plugins CNI

#### Básicos
- bridge
- host-local
- loopback
- dhcp

#### Metaplugins
- flannel
- tuning
- portmap
- bandwidth

#### Plugins Avançados
- Calico
  - BGP networking
  - Network policies
  - IPAM
- Cilium
  - eBPF-based networking
  - L7 policies
  - Observability
- Weave
  - Encrypted networking
  - Service discovery
  - Fast datapath

## Configuração de Rede

### Port Mapping
```bash
# Exposição básica
docker run -p 8080:80 nginx

# UDP ports
docker run -p 53:53/udp dns-server

# Múltiplos ports
docker run \
  -p 80:80 \
  -p 443:443 \
  -p 8080:8080 \
  web-server

# Range de portas
docker run -p 8080-8090:80-90 nginx
```

### DNS e Service Discovery

#### Configuração DNS
```yaml
# /etc/docker/daemon.json
{
  "dns": ["8.8.8.8", "8.8.4.4"],
  "dns-search": ["example.com"],
  "dns-opts": ["ndots:2", "timeout:2"]
}
```

#### Docker Compose
```yaml
version: '3.8'
services:
  web:
    networks:
      frontend:
        aliases:
          - web.local
      backend:
        aliases:
          - web.internal
  api:
    networks:
      backend:
        ipv4_address: 172.16.238.10
        
networks:
  frontend:
    driver: bridge
    enable_ipv6: true
    ipam:
      driver: default
      config:
        - subnet: 172.16.238.0/24
        - subnet: "2001:db8:1::/64"
  backend:
    internal: true
    driver: bridge
```

## Segurança de Rede

### Network Policies

#### Isolamento Básico
```yaml
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

#### Políticas Avançadas
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: api-allow
spec:
  podSelector:
    matchLabels:
      role: api
  policyTypes:
  - Ingress
  - Egress
  ingress:
  - from:
    - namespaceSelector:
        matchLabels:
          purpose: production
    ports:
    - protocol: TCP
      port: 8080
  egress:
  - to:
    - ipBlock:
        cidr: 10.0.0.0/24
        except:
        - 10.0.0.1/32
    ports:
    - protocol: TCP
      port: 5432
```

### Isolamento

#### Técnicas de Isolamento
- Network namespaces
- Bridge isolation
- VLAN tagging
- Security groups

#### Firewall Rules
```bash
# Bloquear acesso externo
iptables -I DOCKER-USER -i ext_if ! -s 192.168.1.0/24 -j DROP

# Permitir apenas portas específicas
iptables -I DOCKER-USER -i ext_if -p tcp --dport 80 -j ACCEPT
iptables -I DOCKER-USER -i ext_if -p tcp --dport 443 -j ACCEPT
```

## Troubleshooting

### Ferramentas de Diagnóstico

#### Network Inspection
```bash
# Detalhes da rede
docker network inspect bridge

# Lista de endpoints
docker network inspect -f '{{range .Containers}}{{.Name}} {{.IPv4Address}}{{end}}' bridge

# Estatísticas de rede
docker stats --no-stream --format "table {{.Container}}\t{{.NetIO}}"
```

#### Debugging Tools
```bash
# tcpdump
docker run --net=container:target \
  --privileged \
  nicolaka/netshoot \
  tcpdump -i any port 80

# netstat
docker exec container netstat -tulpn

# iptables
docker run --net=host --privileged \
  nicolaka/netshoot \
  iptables -nvL
```

### Problemas Comuns

#### Matriz de Troubleshooting
| Problema | Diagnóstico | Solução | Comando |
|----------|------------|----------|---------|
| DNS | `nslookup` | Verificar resolv.conf | `docker exec container cat /etc/resolv.conf` |
| Conectividade | `ping` | Verificar firewall | `iptables -nvL` |
| Portas | `netstat` | Verificar binding | `netstat -tulpn` |
| Performance | `iperf` | Testar bandwidth | `iperf -c target` |

## Monitoramento

### Métricas de Rede

#### Básicas
```bash
# Estatísticas básicas
docker stats --format "table {{.Name}}\t{{.NetIO}}"

# IO detalhado
docker stats --no-stream --format "table {{.Container}}\t{{.NetIO}}\t{{.BlockIO}}"
```

#### Avançadas
```bash
# Monitoramento com cAdvisor
docker run \
  --volume=/:/rootfs:ro \
  --volume=/var/run:/var/run:ro \
  --volume=/sys:/sys:ro \
  --volume=/var/lib/docker/:/var/lib/docker:ro \
  --publish=8080:8080 \
  --detach=true \
  --name=cadvisor \
  google/cadvisor:latest
```

### Logging

#### Configuração de Log
```json
{
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m",
    "max-file": "3"
  }
}
```

#### Coleta de Logs
```bash
# Logs de rede
docker logs --tail 100 -f container

# Eventos de rede
docker events --filter 'type=network'
```

## Referências

### Documentação
- [Docker Networking Overview](https://docs.docker.com/network/)
- [CNI Specification](https://github.com/containernetworking/cni/blob/master/SPEC.md)
- [Libnetwork Design](https://github.com/docker/libnetwork/blob/master/docs/design.md)

### Ferramentas
- [Netshoot](https://github.com/nicolaka/netshoot)
- [Network Debug Tools](https://github.com/docker/docker-diagnose)
- [Network Plugins](https://github.com/containernetworking/plugins)

### Tutoriais Avançados
- [Multi-host Networking](https://docs.docker.com/network/overlay/)
- [Network Security](https://docs.docker.com/network/security/)
- [Custom Network Plugins](https://docs.docker.com/engine/extend/plugins_network/)

### Recursos Adicionais
- [Docker Network Troubleshooting Guide](https://success.docker.com/article/troubleshooting-container-networking)
- [Container Network Interface Specification](https://github.com/containernetworking/cni/blob/master/SPEC.md)
- [Docker Network Architecture](https://docs.docker.com/engine/userguide/networking/dockernetworks/)