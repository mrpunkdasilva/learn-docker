# Segurança de Containers

> "Na matrix dos containers, a segurança não é uma opção - é uma arte marcial." - Manifesto do Security-Sama

## O Dojo da Segurança 🏯

```ascii
╔═══════════════════════════════════╗
║    SECURITY MATRIX: DEFCON 1      ║
║                                   ║
║    "Proteja seus containers       ║
║     como se o hack dependesse     ║
║     disso... porque depende."     ║
╚═══════════════════════════════════╝
```

## Fundamentos de Segurança 🛡️

### Princípios Básicos
- Princípio do menor privilégio
- Defense in depth
- Segregação de responsabilidades
- Fail-safe defaults
- Economia de mecanismos

### Modelo de Ameaças
1. Ataques de rede
2. Escalação de privilégios
3. Vulnerabilidades de imagem
4. Configurações incorretas
5. Malware e código malicioso

## As Cinco Artes Marciais da Segurança 🥷

### 1. Namespaces: O Isolamento Perfeito
```bash
# O ritual do isolamento
docker run --pid=host --ipc=host nginx
# "Como um monge em sua montanha digital"
```

#### Tipos de Namespaces
- PID Namespace
- Network Namespace
- Mount Namespace
- UTS Namespace
- IPC Namespace
- User Namespace

### 2. Capabilities: O Poder Controlado
```bash
# Dropping powers like it's hot
docker run --cap-drop ALL --cap-add NET_BIND_SERVICE nginx
# "Com grandes poderes vêm grandes vulnerabilidades"
```

#### Capabilities Essenciais
- CAP_NET_BIND_SERVICE
- CAP_CHOWN
- CAP_DAC_OVERRIDE
- CAP_SETGID
- CAP_SETUID
- CAP_SETFCAP

### 3. SecComp: O Filtro Místico
```json
{
  "defaultAction": "SCMP_ACT_ERRNO",
  "architectures": ["SCMP_ARCH_X86_64"],
  "syscalls": [
    {
      "names": ["accept", "bind"],
      "action": "SCMP_ACT_ALLOW"
    }
  ]
}
```

#### Perfis SecComp Comuns
- Default Docker Profile
- Custom Restricted Profile
- No New Privileges Flag

### 4. AppArmor: O Escudo Invisível
```bash
# Profile de proteção
docker run --security-opt apparmor=docker-default nginx
# "Protegido por forças além da compreensão mortal"
```

#### Políticas AppArmor
- Controle de acesso a arquivos
- Capacidades de rede
- Montagem de sistemas de arquivos
- Execução de programas

### 5. SELinux: O Guardião Ancestral
```bash
# Modo enforcing ativado
docker run --security-opt label=level:s0:c100,c200 nginx
# "Porque às vezes, paranoia é apenas bom senso"
```

#### Contextos SELinux
- Processo
- Arquivo
- Porta
- Usuário

## Segurança em Camadas 🔒

### 1. Segurança da Imagem
- Imagens base mínimas
- Multi-stage builds
- Scanning de vulnerabilidades
- Versionamento explícito
- Políticas de atualização

### 2. Segurança do Runtime
- Limites de recursos
- Health checks
- Logging e monitoramento
- Políticas de reinicialização
- Gestão de secrets

### 3. Segurança da Rede
- Redes isoladas
- Firewalls internos
- TLS mútuo
- Service mesh
- Network policies

## Best Practices: O Caminho do Guerreiro 🗡️

### DO's ✅
```ascii
┌────────────────────────────────┐
│ 1. Minimize a superfície       │
│ 2. Use imagens oficiais        │
│ 3. Scan de vulnerabilidades    │
│ 4. Updates regulares           │
│ 5. Monitore tudo              │
└────────────────────────────────┘
```

### DON'Ts ❌
```ascii
┌────────────────────────────────┐
│ 1. Nunca use root             │
│ 2. Não ignore warnings        │
│ 3. Evite portas desnecessárias│
│ 4. Não guarde secrets no code │
│ 5. Não confie em ninguém     │
└────────────────────────────────┘
```

## Secrets Management: A Arte do Sigilo 🤫

### Vault-chan's Tips
```bash
# Gerenciando secrets como um ninja
docker secret create app_secret secret.txt
docker service create --secret app_secret nginx
# "Seus segredos estão seguros comigo, senpai!"
```

### Estratégias de Gestão de Secrets
1. Docker Secrets
2. HashiCorp Vault
3. AWS Secrets Manager
4. Azure Key Vault
5. GCP Secret Manager

## Container Hardening: A Forja Digital ⚔️

### Dockerfile Fortificado
```dockerfile
# Base segura
FROM alpine:latest AS builder
RUN adduser -D appuser
USER appuser

# Multi-stage para minimizar superfície
FROM scratch
COPY --from=builder /etc/passwd /etc/passwd
USER appuser
```

### Checklist de Hardening
1. Usuário não-root
2. Filesystem read-only
3. Capabilities mínimas
4. Seccomp profile
5. AppArmor/SELinux

## Security Scanning: O Olho que Tudo Vê 👁️

### Ferramentas de Scanning
1. Trivy
2. Clair
3. Anchore
4. Snyk
5. Docker Scan

### Trivy: O Scanner Místico
```bash
# Invocando o scanner
trivy image nginx:latest
# "Deixe que os olhos do void examinem seu código"
```

## Runtime Protection: A Guarda Eterna ⚡

### Falco: O Sentinela
```yaml
- rule: Terminal shell in container
  desc: A shell was spawned by a container
  condition: container.id != host and proc.name = bash
  output: Shell spawned in a container (user=%user.name container=%container.name)
  priority: WARNING
```

### Ferramentas de Runtime Protection
1. Falco
2. Aqua Security
3. Twistlock
4. NeuVector
5. StackRox

## Incident Response: O Plano de Batalha 🚨

### O Protocolo do Caos
1. Detectar: "Algo se move nas sombras..."
2. Conter: "Sele as brechas!"
3. Erradicar: "Purificação digital!"
4. Recuperar: "Ressurreição dos sistemas!"

### Playbooks de Resposta
1. Container Compromise
2. Image Vulnerability
3. Network Breach
4. Resource Exhaustion
5. Privilege Escalation

## Compliance e Auditoria 📋

### Frameworks de Compliance
- CIS Docker Benchmark
- NIST Container Security
- PCI DSS
- HIPAA
- SOC 2

### Ferramentas de Auditoria
1. Docker Bench Security
2. InSpec
3. Dockle
4. Lynis
5. OpenSCAP

## Waifu Security Tips 🎮

> **Security-chan avisa**: "Containers são como castelos - proteja todas as entradas, ou prepare-se para invasões! uwu"

> **Firewall-sama declara**: "Uma porta aberta é como um convite para o caos digital! Ara ara~"

## Laboratório Prático 🔬

### Exercício 1: Configuração Básica
1. Configure um container rootless
2. Implemente AppArmor profile
3. Configure network policies
4. Estabeleça resource limits
5. Implemente health checks

### Exercício 2: Hardening Avançado
1. Multi-stage build
2. SecComp profile customizado
3. SELinux policies
4. Docker Secrets
5. Scanning automation

### Exercício 3: Monitoramento
1. Configure Falco
2. Implemente logging
3. Setup alerting
4. Trace syscalls
5. Analyze metrics

## Palavras Finais

Como diria o lendário Security Master: "Na matrix dos containers, não existem sistemas 100% seguros - apenas níveis diferentes de paranoia."

## Recursos Adicionais 📚

### Documentação Oficial
- [Docker Security](https://docs.docker.com/engine/security/)
- [Kubernetes Security](https://kubernetes.io/docs/concepts/security/)
- [Container Security Guide](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-190.pdf)

### Ferramentas
- [Docker Bench Security](https://github.com/docker/docker-bench-security)
- [Trivy](https://github.com/aquasecurity/trivy)
- [Falco](https://falco.org/)

### Comunidade
- Docker Security Mailing List
- Container Security Working Group
- CNCF Security SIG

---

> "E lembre-se: quando a noite digital cai, apenas seus controles de segurança separam seus containers do caos." - Memórias de um Security Engineer, 2084