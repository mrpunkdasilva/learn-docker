# Containers 101

```ascii
╔═══════════════════════════════════════════════════════════════════════╗
║  CONTAINER BASICS MATRIX                                              ║
║                                                                       ║
║  [ Container = Aplicação + Dependências + Runtime ]                   ║
║                                                                       ║
║  "Tudo que você precisa, nada que você não precisa"                  ║
║                                                                       ║
╚═══════════════════════════════════════════════════════════════════════╝
```

## O que é um Container? 📦

Um container é como uma cápsula auto-suficiente que contém:
- Sua aplicação
- Bibliotecas necessárias
- Configurações
- Runtime environment

```ascii
┌──────────────────────┐
│    Sua Aplicação     │
├──────────────────────┤
│    Dependências      │
├──────────────────────┤
│      Runtime         │
└──────────────────────┘
```

## Anatomia de um Container 🔬

### Componentes Principais
1. **Namespace** - Isolamento de processos
2. **Cgroups** - Controle de recursos
3. **Union Filesystem** - Sistema de arquivos em camadas
4. **Container Runtime** - Motor de execução

## Por Que Usar Containers? 🤔

### Benefícios
- Isolamento consistente
- Portabilidade garantida
- Deploy simplificado
- Escalabilidade fácil
- Versionamento eficiente

## Waifu Tips 💡

> **Container-chan diz:** "Lembre-se: containers são efêmeros! Se algo importante mudar, será perdido no próximo restart... como lágrimas na chuva!"
> {style="note"}

## Conceitos Fundamentais 📚

### 1. Imutabilidade
- Containers não devem ser modificados em runtime
- Mudanças através de novas builds
- Tratados como gado, não como pets

### 2. Isolamento
```ascii
┌────────────App 1─────┐  ┌────────────App 2─────┐
│  ┌──────────────┐    │  │   ┌──────────────┐   │
│  │   Process    │    │  │   │   Process    │   │
│  └──────────────┘    │  │   └──────────────┘   │
│         ▼            │  │          ▼            │
│  ┌──────────────┐    │  │   ┌──────────────┐   │
│  │  Filesystem  │    │  │   │  Filesystem  │   │
│  └──────────────┘    │  │   └──────────────┘   │
└────────────────────┘  └────────────────────────┘
```

### 3. Recursos Limitados
- CPU controlada
- Memória definida
- I/O gerenciado
- Network isolada

## Primeiros Passos 👣

### Hello World Container
```bash
# Rode seu primeiro container
docker run hello-world

# Veja o que está rodando
docker ps

# Veja todos os containers
docker ps -a
```

### Container Interativo
```bash
# Ubuntu container interativo
docker run -it ubuntu bash

# Alpine container leve
docker run -it alpine sh
```

## Container Lifecycle 🔄

```ascii
┌──────────┐    ┌──────────┐    ┌──────────┐
│ Created  │ ─► │ Running  │ ─► │ Stopped  │
└──────────┘    └──────────┘    └──────────┘
      ▲                              │
      └──────────────────────────────┘
```

### Comandos de Ciclo de Vida
```bash
# Criar container
docker create nginx

# Iniciar container
docker start <container-id>

# Parar container
docker stop <container-id>

# Remover container
docker rm <container-id>
```

## Práticas Recomendadas 🎯

### DO's ✅
- Um processo por container
- Use tags específicas
- Defina limites de recursos
- Monitore seus containers
- Use health checks

### DON'Ts ❌
- Não armazene dados no container
- Evite containers muito grandes
- Não use `latest` tag
- Não rode como root
- Não ignore security scans

## Exercícios Práticos 🏋️

1. **Hello Container**
   ```bash
   # Rode e entenda o hello-world
   docker run hello-world
   ```

2. **Container Persistente**
   ```bash
   # Rode nginx com porta exposta
   docker run -d -p 8080:80 nginx
   ```

3. **Container Exploração**
   ```bash
   # Entre em um container rodando
   docker exec -it <container-id> bash
   ```

## Debug Básico 🔍

### Comandos Essenciais
```bash
# Logs do container
docker logs <container-id>

# Processos rodando
docker top <container-id>

# Estatísticas de uso
docker stats <container-id>
```

## Checkpoint ✅

Você agora sabe:
- [x] O que são containers
- [x] Como eles funcionam
- [x] Comandos básicos
- [x] Boas práticas
- [x] Ciclo de vida

## Próximos Passos 🎯

1. Explore Docker Hub
2. Crie seus próprios containers
3. Aprenda sobre networking
4. Estude volumes
5. Pratique com projetos reais

---

> "No mundo dos containers, não existe 'funciona na minha máquina' - porque sua máquina está no container!"

## Deep Dive: Camadas de um Container 🧅

### Union File System
```ascii
┌────────────────────────┐
│    Camada R/W          │ <- Container Layer
├────────────────────────┤
│    Aplicação           │ <- Image Layer 3
├────────────────────────┤
│    Dependências        │ <- Image Layer 2
├────────────────────────┤
│    Sistema Base        │ <- Image Layer 1
└────────────────────────┘
```

### Como as Camadas Funcionam
1. **Base Layer**: Sistema operacional mínimo
2. **Middleware Layer**: Runtime, libs
3. **Application Layer**: Seu código
4. **Container Layer**: Mudanças em runtime

## Networking Básico 🌐

### Tipos de Rede
```bash
# Lista redes disponíveis
docker network ls

# Cria rede customizada
docker network create minha-rede

# Conecta container à rede
docker network connect minha-rede container-id
```

### Modelos de Rede
1. **Bridge** (default)
   ```bash
   docker run --network bridge nginx
   ```

2. **Host** (usa rede do host)
   ```bash
   docker run --network host nginx
   ```

3. **None** (sem rede)
   ```bash
   docker run --network none nginx
   ```

## Storage & Volumes 💾

### Tipos de Storage
1. **Volumes** (gerenciado pelo Docker)
   ```bash
   # Cria volume
   docker volume create meu-volume
   
   # Usa volume
   docker run -v meu-volume:/data nginx
   ```

2. **Bind Mounts** (path do host)
   ```bash
   # Monta diretório local
   docker run -v $(pwd):/app nginx
   ```

3. **tmpfs** (memória)
   ```bash
   # Armazenamento temporário
   docker run --tmpfs /app nginx
   ```

## Container Security 🔒

### Boas Práticas de Segurança
1. **Rootless Containers**
   ```dockerfile
   # No Dockerfile
   USER non-root-user
   ```

2. **Scan de Vulnerabilidades**
   ```bash
   # Scan de imagem
   docker scan nginx:latest
   ```

3. **Limites de Recursos**
   ```bash
   # Limita CPU e memória
   docker run --cpus=.5 --memory=512m nginx
   ```

## Advanced Commands 🚀

### Container Stats
```bash
# Stats em tempo real
docker stats --format "table {{.Name}}\t{{.CPUPerc}}\t{{.MemUsage}}"

# Info detalhada
docker inspect container-id
```

### Batch Operations
```bash
# Para todos containers
docker stop $(docker ps -q)

# Remove containers parados
docker container prune

# Remove tudo
docker system prune -a
```

## Troubleshooting Matrix 🔍

### Common Issues
| Problema | Comando de Debug | Solução |
|----------|-----------------|----------|
| Container não inicia | `docker logs` | Verifique logs |
| Porta em uso | `docker port` | Mude a porta |
| Sem espaço | `docker system df` | Limpe recursos |

### Debug Commands
```bash
# Histórico de comandos
docker history imagem:tag

# Eventos em tempo real
docker events

# Diff de alterações
docker diff container-id
```

## Container Development Workflow 🔄

### Local Development
1. **Build Local**
   ```bash
   docker build -t app:dev .
   ```

2. **Hot Reload**
   ```bash
   docker run -v $(pwd):/app app:dev
   ```

3. **Debug Mode**
   ```bash
   docker run --rm -it app:dev sh
   ```

### Multi-container Setup
```yaml
# docker-compose.yml
version: '3.8'
services:
  app:
    build: .
    ports:
      - "3000:3000"
  db:
    image: postgres
    environment:
      POSTGRES_PASSWORD: secret
```

## Performance Tuning 🏎️

### Otimizações
1. **Multi-stage Builds**
   ```dockerfile
   FROM node:alpine AS builder
   WORKDIR /app
   COPY . .
   RUN npm run build

   FROM nginx:alpine
   COPY --from=builder /app/dist /usr/share/nginx/html
   ```

2. **Cache Optimization**
   ```dockerfile
   COPY package*.json ./
   RUN npm install
   COPY . .
   ```

3. **Resource Limits**
   ```bash
   docker run \
     --cpus=2 \
     --memory=2g \
     --memory-reservation=1g \
     --memory-swap=4g \
     nginx
   ```

## Waifu Advanced Tips 🎮

> **Container-chan Advanced Tip:** "Multi-stage builds são como fazer cosplay - você usa diferentes outfits (stages) para chegar no resultado final perfeito!"
> {style="tip"}

> **Security-chan Warning:** "Nunca confie em imagens de fontes desconhecidas - é como aceitar doces de estranhos!"
> {style="warning"}

## Container Patterns 📋

### Sidecar Pattern
```ascii
┌────────────────────┐
│  Main Container    │◄──┐
└────────────────────┘   │
                         │
┌────────────────────┐   │
│  Sidecar Container │───┘
└────────────────────┘
```

### Ambassador Pattern
```ascii
┌────────────────────┐   ┌────────────────┐
│  Main Container    │◄──│   Ambassador   │
└────────────────────┘   └────────────────┘
```

## Exercícios Avançados 🏋️‍♂️

1. **Multi-container App**
   - Crie uma aplicação web com frontend, backend e database
   - Use docker-compose
   - Implemente volumes persistentes

2. **Custom Network**
   - Configure rede bridge customizada
   - Conecte múltiplos containers
   - Teste comunicação entre containers

3. **Resource Monitoring**
   - Configure limites de recursos
   - Monitore uso com docker stats
   - Implemente health checks

## Certificação Checkpoint 📝

Prepare-se para certificações Docker com estes conceitos:
- [x] Container internals
- [x] Networking modes
- [x] Storage options
- [x] Security best practices
- [x] Resource management
- [x] Troubleshooting
- [x] Container patterns

## Links Úteis 🔗

- [Docker Hub](https://hub.docker.com)
- [Docker Docs](https://docs.docker.com)
- [Play with Docker](https://labs.play-with-docker.com)

---

> "Um container bem configurado é como um bom código - ele faz exatamente o que deveria fazer, nada mais, nada menos."

## Stateful vs Stateless 🔄

### Stateless Containers
```ascii
┌────────────────────┐
│  Stateless App     │
│  ┌──────────────┐  │
│  │   Request    │  │
│  │   Process    │──┼─► Response
│  │   Response   │  │
│  └──────────────┘  │
└────────────────────┘
     No State ❌
```

#### Características
- Não mantém estado entre requisições
- Facilmente escalável
- Pode ser substituído a qualquer momento
- Ideal para microserviços

#### Exemplos de Uso
- Web servers
- API endpoints
- Processamento de filas
- Transformação de dados

### Stateful Containers
```ascii
┌────────────────────┐
│   Stateful App     │
│  ┌──────────────┐  │
│  │   Process    │  │
│  └──────┬───────┘  │
│         ▼          │
│  ┌──────────────┐  │
│  │    State     │  │
│  └──────────────┘  │
└────────────────────┘
    Persistent ✅
```

#### Características
- Mantém dados entre restarts
- Requer volumes persistentes
- Mais complexo para escalar
- Precisa de backup strategy

#### Exemplos de Uso
- Databases
- Cache systems
- Message queues
- Session stores

## Containers vs VMs: Análise Detalhada 🔍

### Comparação de Arquitetura
```ascii
┌─────────Containers─────────┐  ┌──────────VMs──────────┐
│ ┌────┐ ┌────┐ ┌────┐      │  │ ┌────┐ ┌────┐ ┌────┐ │
│ │App1│ │App2│ │App3│      │  │ │App1│ │App2│ │App3│ │
│ └────┘ └────┘ └────┘      │  │ └────┘ └────┘ └────┘ │
│     Container Engine       │  │ ┌────┐ ┌────┐ ┌────┐ │
│           │               │  │ │ OS │ │ OS │ │ OS │ │
│     Host Kernel           │  │ └────┘ └────┘ └────┘ │
└───────────────────────────┘  │      Hypervisor      │
                               └─────────────────────────┘
```

### Matriz de Comparação

| Aspecto          | Containers              | VMs                    |
|------------------|------------------------|------------------------|
| Boot Time        | Segundos               | Minutos                |
| Tamanho          | MBs                    | GBs                    |
| Performance      | Próximo ao bare metal  | Overhead significativo |
| Isolamento       | Processo-level         | Hardware-level         |
| Portabilidade    | Muito alta             | Limitada               |
| Segurança        | Compartilha kernel     | Totalmente isolada     |
| Densidade        | Alta (100s)            | Baixa (10s)           |

## Use Cases Detalhados 🎯

### Ideal para Containers

1. **Microserviços**
```ascii
┌────────────────────────────────────────┐
│    Microservices Architecture          │
│ ┌────┐ ┌────┐ ┌────┐ ┌────┐ ┌────┐     │
│ │API │ │Auth│ │Cart│ │User│ │Pay │     │
│ └────┘ └────┘ └────┘ └────┘ └────┘     │
└────────────────────────────────────────┘
```

2. **CI/CD Pipelines**
```ascii
┌─────┐   ┌──────┐   ┌─────┐   ┌──────┐
│Build│ ► │Test  │ ► │Stage│ ► │Deploy│
└─────┘   └──────┘   └─────┘   └──────┘
```

3. **Desenvolvimento Local**
```ascii
┌────────────────────┐
│  Dev Environment   │
│ ┌────┐ ┌────────┐ │
│ │App │ │Database│ │
│ └────┘ └────────┘ │
└────────────────────┘
```

### Ideal para VMs

1. **Legacy Applications**
```ascii
┌────────────────────────┐
│    Monolithic App      │
│ ┌──────────────────┐   │
│ │  Full Stack App  │   │
│ │  + Dependencies  │   │
│ │  + Full OS      │   │
│ └──────────────────┘   │
└────────────────────────┘
```

2. **Isolamento Completo**
```ascii
┌────────────────────┐
│   Secure VM        │
│ ┌───────────────┐  │
│ │ Encrypted     │  │
│ │ Environment   │  │
│ └───────────────┘  │
└────────────────────┘
```

## Padrões de Deployment 🚀

### Blue-Green Deployment
```ascii
┌────────Blue───────┐   ┌────────Green──────┐
│  Version 1.0      │   │  Version 1.1      │
│  ┌────────────┐   │   │  ┌────────────┐   │
│  │ Container  │   │   │  │ Container  │   │
│  └────────────┘   │   │  └────────────┘   │
└──────────┬────────┘   └──────────┬────────┘
           │                       │
    ┌──────┴───────────────────────┴───────┐
    │           Load Balancer              │
    └────────────────────────────────────┬─┘
                                         │
                                      Traffic
```

### Rolling Updates
```ascii
┌────────────────────────────────────────┐
│    V1    │    V1    │    V2    │    V2 │
├──────────┼──────────┼──────────┼───────┤
│    V1    │    V2    │    V2    │    V2 │
└────────────────────────────────────────┘
     Time ───────────────────────►
```

## Estratégias de Persistência 💾

### Volume Types
```ascii
┌────────────────────┐
│    Container       │
│  ┌──────────────┐  │
│  │  Application │  │
│  └──────┬───────┘  │
│         ▼          │
│  ┌──────────────┐  │
│  │   Volume     │──┼──► Host Storage
│  └──────────────┘  │
└────────────────────┘
```

1. **Named Volumes**
```bash
docker volume create mydata
docker run -v mydata:/data nginx
```

2. **Bind Mounts**
```bash
docker run -v /host/path:/container/path nginx
```

3. **tmpfs Mounts**
```bash
docker run --tmpfs /tmp nginx
```

## Networking Avançado 🌐

### Network Types
```ascii
┌────────────────────┐
│    Container A     │
│     172.17.0.2    │
└──────────┬─────────┘
           │
┌──────────┴─────────┐
│  Docker Network    │
│    172.17.0.0/16  │
└──────────┬─────────┘
           │
┌──────────┴─────────┐
│    Container B     │
│     172.17.0.3    │
└────────────────────┘
```

### Network Modes
1. **Bridge Network**
   - Default network driver
   - Isolated network
   - Port mapping

2. **Host Network**
   - Uses host networking
   - Better performance
   - Less isolation

3. **Overlay Network**
   - Multi-host networking
   - Swarm mode
   - Service discovery

## Monitoramento e Logging 📊

### Monitoring Stack
```ascii
┌────────────────┐    ┌────────────────┐
│   Container    │    │   Prometheus   │
│  ┌──────────┐ │    │  Metrics DB    │
│  │ App      │ │    │                │
│  │ +Metrics │ │───►│  ┌──────────┐  │
│  └──────────┘ │    │  │ Grafana  │  │
└────────────────┘    │  └──────────┘  │
                      └────────────────┘
```

### Logging Patterns
1. **Stdout/Stderr**
```bash
docker logs container_id
```

2. **Sidecar Logger**
```yaml
services:
  app:
    image: myapp
  logger:
    image: fluentd
    volumes:
      - /var/log:/logs
```

## Waifu Tips 🎮 {id="waifu-tips_1"}

> **State-chan diz:** "Stateless é como um café instantâneo - rápido, prático e sempre igual. Stateful é como um café gourmet - precisa de mais cuidado, mas vale a pena!"
> {style="tip"}

> **VM-sama avisa:** "VMs são como apartamentos completos, containers são como quartos de hotel - escolha baseado no que precisa!"
> {style="note"}

## Próximos Passos 🎯 {id="pr-ximos-passos_1"}

1. Explore Docker Hub
2. Crie seus próprios containers
3. Aprenda sobre networking
4. Estude volumes
5. Pratique com projetos reais

---

> "No mundo dos containers, não existe 'funciona na minha máquina' - porque sua máquina está no container!"
