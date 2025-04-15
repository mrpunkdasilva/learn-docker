# Container Internals: Como Funciona Por Dentro 🔬

```ascii
╔═══════════════════════════════════════════════════════════════════════╗
║  CONTAINER INTERNALS MATRIX                                           ║
║                                                                       ║
║  [ Namespaces + Cgroups + UnionFS = Container ]                      ║
║                                                                       ║
║  "Isolamento, Controle e Eficiência"                                 ║
║                                                                       ║
╚═══════════════════════════════════════════════════════════════════════╝
```

## Linux Namespaces 🏰

### Tipos de Namespaces Detalhados

#### 1. PID Namespace
```ascii
┌─────────────────────────┐
│ Host PID Namespace      │
│  pid: 1234             │
│  ┌──────────────────┐  │
│  │Container PID NS  │  │
│  │  pid: 1         │  │
│  └──────────────────┘  │
└─────────────────────────┘
```

- Isolamento de processos
- Hierarquia própria
- PID 1 como init
- Nested namespaces

#### 2. Network Namespace
```ascii
┌────────────────────────────┐
│ Host Network               │
│ eth0: 192.168.1.10        │
│  ┌──────────────────────┐  │
│  │Container Net NS      │  │
│  │ eth0: 172.17.0.2    │  │
│  │ lo: 127.0.0.1       │  │
│  └──────────────────────┘  │
└────────────────────────────┘
```

- Interface virtual
- Routing tables próprias
- Firewall rules
- Socket isolation

#### 3. Mount Namespace
```ascii
┌────────────────────────┐
│ Host Mount NS          │
│  /                     │
│  ├── bin              │
│  ├── etc              │
│  └── container_root    │
│      └── [Container]  │
└────────────────────────┘
```

- Filesystem isolation
- Bind mounts
- Pivoting root
- Mount propagation

## Control Groups (cgroups) v2 🎛️

### Hierarquia Unificada
```ascii
┌─────────────────────────────────┐
│ /sys/fs/cgroup/                 │
├─────────────────────────────────┤
│ ├── container.slice            │
│ │   ├── memory.max            │
│ │   ├── cpu.max               │
│ │   └── io.max                │
│ └── system.slice              │
└─────────────────────────────────┘
```

### Controllers Detalhados

#### CPU Controller
```bash
# Limitar CPU para 50%
echo "50000 100000" > cpu.max

# Definir peso CPU
echo "100" > cpu.weight
```

#### Memory Controller
```bash
# Limite de memória
echo "256M" > memory.max

# Soft limit
echo "200M" > memory.high
```

#### I/O Controller
```bash
# Limite de I/O
echo "10485760" > io.max

# Peso I/O
echo "100" > io.weight
```

## Union File System Deep Dive 📦

### Overlay2 Internals
```ascii
┌────────────────────────────┐
│ Container Layer (Upper)    │
├────────────────────────────┤
│ Container Write Layer      │
├────────────────────────────┤
│ Image Layer N (Lower)      │
├────────────────────────────┤
│ Image Layer 2 (Lower)      │
├────────────────────────────┤
│ Base Layer (Lower)         │
└────────────────────────────┘
```

### Layer Operations
```ascii
┌────────────────────────┐
│ Write Operation        │
│ ┌──────────────────┐   │
│ │ Copy-Up          │   │
│ │ Modify Upper     │   │
│ └──────────────────┘   │
└────────────────────────┘
```

### Directory Structure
```bash
/var/lib/docker/overlay2/
├── l/                  # Layer shortcuts
├── [layer-id]/         # Layer content
│   ├── diff/          # Layer fs content
│   ├── work/          # Overlay work dir
│   ├── merged/        # Mount point
│   └── lower          # Lower layer refs
```

## Container Runtime Spec 🏃

### OCI Runtime Specification
```ascii
┌────────────────────────────┐
│ runtime-spec               │
├────────────────────────────┤
│ ├── config.json          │
│ ├── rootfs/              │
│ └── state.json          │
└────────────────────────────┘
```

### Container Lifecycle States
```ascii
┌──────────┐     ┌──────────┐
│ creating │ ──► │ created  │
└──────────┘     └────┬─────┘
                      │
                      ▼
┌──────────┐     ┌──────────┐
│ stopped  │ ◄── │ running  │
└──────────┘     └──────────┘
```

## Security Internals 🔒

### Linux Capabilities
```ascii
┌────────────────────────────────┐
│ Default Container Capabilities │
├────────────────────────────────┤
│ CHOWN          │ SETUID       │
│ DAC_OVERRIDE   │ SETGID       │
│ FOWNER         │ NET_BIND     │
│ MKNOD          │ SYS_CHROOT   │
└────────────────────────────────┘
```

### Seccomp Profiles
```json
{
  "defaultAction": "SCMP_ACT_ERRNO",
  "architectures": ["SCMP_ARCH_X86_64"],
  "syscalls": [
    {
      "names": ["read", "write"],
      "action": "SCMP_ACT_ALLOW"
    }
  ]
}
```

### AppArmor Profiles
```ascii
┌────────────────────────────┐
│ Docker Default Profile     │
├────────────────────────────┤
│ file,                     │
│ network,                  │
│ capability,               │
│ mount                     │
└────────────────────────────┘
```

## Advanced Networking 🌐

### CNI (Container Network Interface)
```ascii
┌────────────────────────────┐
│ CNI Architecture          │
├────────────────────────────┤
│ ┌──────────┐ ┌──────────┐ │
│ │ Plugin   │ │ IPAM     │ │
│ └──────────┘ └──────────┘ │
└────────────────────────────┘
```

### Network Drivers
```ascii
┌────────────────────────────┐
│ Network Driver Types       │
├────────────────────────────┤
│ bridge                    │
│ host                      │
│ overlay                   │
│ macvlan                   │
│ none                      │
└────────────────────────────┘
```

## Storage Deep Dive 💾

### Storage Driver Operations
```ascii
┌────────────────────────────┐
│ Storage Driver Stack       │
├────────────────────────────┤
│ Container Layer           │
│ Image Layers              │
│ Storage Driver           │
│ Block Device             │
└────────────────────────────┘
```

### Volume Types
```ascii
┌────────────────────────────┐
│ Volume Types               │
├────────────────────────────┤
│ Named Volumes             │
│ Bind Mounts               │
│ tmpfs Mounts             │
└────────────────────────────┘
```

## Advanced Debugging 🔍

### Trace Commands
```bash
# Syscall tracing
strace -p $PID

# Network tracing
tcpdump -i any container_port

# Block I/O tracing
blktrace -d /dev/sda -o trace
```

### Profiling
```bash
# CPU profiling
perf record -p $PID

# Memory profiling
heaptrack $PID
```

## Performance Tuning ⚡

### Resource Optimization
```ascii
┌────────────────────────────┐
│ Performance Factors        │
├────────────────────────────┤
│ CPU Scheduling            │
│ Memory Management         │
│ I/O Throughput           │
│ Network Latency          │
└────────────────────────────┘
```

### Monitoring Metrics
```bash
# CPU stats
docker stats --format "table {{.Container}}\t{{.CPUPerc}}"

# Memory usage
docker stats --format "table {{.Container}}\t{{.MemUsage}}"
```

## Waifu Tips 🎮

> **Runtime-chan diz:** "OCI é como um contrato mágico que garante que todos os containers falem a mesma língua!"
> {style="tip"}

> **Security-sama avisa:** "Capabilities são como superpoderes - use com responsabilidade!"
> {style="warning"}

> **Network-chan lembra:** "CNI é como um protocolo de comunicação entre dimensões paralelas!"
> {style="note"}

## Referências Técnicas 📚

### Documentação Oficial
- OCI Runtime Specification
- Linux Kernel Documentation
- Docker Engine Internals
- Container Security Guide

### Ferramentas Úteis
- nsenter
- unshare
- runc
- ctr
- nerdctl

### Debugging Tools
- strace
- tcpdump
- perf
- heaptrack
