# Arquitetura Docker: O Blueprint da Matrix 🏗️

```ascii
╔═══════════════════════════════════════════════════════════════════════╗
║  DOCKER ARCHITECTURE BLUEPRINT                                        ║
║                                                                       ║
║  Client → Docker Host → Container Runtime → Registry                  ║
║                                                                       ║
║  "A arquitetura que sustenta o multiverso dos containers"            ║
║                                                                       ║
╚═══════════════════════════════════════════════════════════════════════╝
```

## Componentes Principais 🔧

### 1. Docker Client
```ascii
┌─────────────────────┐
│    Docker CLI       │
├─────────────────────┤
│ - docker build     │
│ - docker pull      │
│ - docker run       │
│ - docker push      │
└─────────────────────┘
```

### 2. Docker Host
```ascii
┌──────────────────────────────────────┐
│           Docker Daemon              │
├──────────────────────────────────────┤
│ ┌────────────┐  ┌────────────────┐  │
│ │ Containers │  │     Images     │  │
│ └────────────┘  └────────────────┘  │
│ ┌────────────┐  ┌────────────────┐  │
│ │  Volumes   │  │    Networks    │  │
│ └────────────┘  └────────────────┘  │
└──────────────────────────────────────┘
```

### 3. Container Runtime
```ascii
┌─────────────────────────────────┐
│ High-Level Runtime (Docker)     │
├─────────────────────────────────┤
│ Container Engine (containerd)   │
├─────────────────────────────────┤
│ Container Runtime (runc)        │
├─────────────────────────────────┤
│ Linux Kernel                    │
└─────────────────────────────────┘
```

### 4. Registry
```ascii
┌────────────────────┐
│  Docker Registry   │
├────────────────────┤
│ - Docker Hub      │
│ - Private Registry│
│ - Mirror Registry │
└────────────────────┘
```

## Fluxo de Comunicação 🔄

### Build Flow
```ascii
[Client] ──build──> [Daemon] ──create──> [Image]
   │                   │                   │
   └───────────────────┴───────push───────┘
                      │
                   [Registry]
```

### Run Flow
```ascii
[Client] ──run──> [Daemon] ──pull──> [Registry]
                    │
                    v
              [Container]
```

## Componentes Detalhados 🔍

### Docker Daemon (dockerd)
- Gerencia objetos Docker
- Manipula API requests
- Gerencia containers
- Controla imagens

### containerd
- Gerenciamento de runtime
- Execução de containers
- Gerenciamento de imagens
- Interface com runc

### runc
- Criação de containers
- Implementação OCI
- Interface com kernel
- Isolamento de recursos

## Waifu Tech Tips 💡

> **Architecture-chan diz:** "Cada componente tem seu papel! Como em um time de idol, todos precisam trabalhar juntos! 🎤"
{style="tip"}

> **Runtime-sama alerta:** "Nunca subestime o poder do container runtime - ele é o verdadeiro MVP! 🏆"
{style="warning"}

## Diagrama de Interação 📊

```ascii
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│  Docker CLI  │────>│Docker Daemon │────>│  containerd  │
└──────────────┘     └──────────────┘     └──────────────┘
                           │                      │
                           v                      v
                    ┌──────────────┐     ┌──────────────┐
                    │   Registry   │     │    runc      │
                    └──────────────┘     └──────────────┘
```

## Segurança e Isolamento 🔒

### Namespace Isolation
- PID Namespace
- Network Namespace
- Mount Namespace
- UTS Namespace
- IPC Namespace
- User Namespace

### Control Groups (cgroups)
- CPU
- Memory
- I/O
- Network
- Devices

## Networking Architecture 🌐

### Default Networks
```ascii
┌─────────────────┐
│     bridge      │
├─────────────────┤
│      host       │
├─────────────────┤
│      none       │
└─────────────────┘
```

### Custom Networks
- Bridge
- Overlay
- Macvlan
- IPvlan

## Storage Architecture 💾

### Storage Drivers
- overlay2
- devicemapper
- btrfs
- zfs
- vfs

### Volume Types
```ascii
┌─────────────────┐
│ Named Volumes   │
├─────────────────┤
│ Bind Mounts     │
├─────────────────┤
│ tmpfs Mounts    │
└─────────────────┘
```

## Checkpoint ✅

Você agora entende:
- [x] Componentes principais
- [x] Fluxo de comunicação
- [x] Container runtime
- [x] Networking
- [x] Storage
- [x] Segurança

## Próximos Passos 🎯

1. [Container Runtime](container-runtime.md)
2. [Networking](container-networking.md)
3. [Storage](container-storage.md)

> "A arquitetura é como um bonsai - cada componente precisa ser cuidadosamente podado e mantido."
> - Docker Architect-sensei, 2025

---

## Referências 📚

- [Docker Documentation](https://docs.docker.com)
- [OCI Specification](https://opencontainers.org)
- [containerd Documentation](https://containerd.io)