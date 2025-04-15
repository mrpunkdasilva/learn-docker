# Container Runtime: O Motor dos Containers 🚀

```ascii
╔═══════════════════════════════════════════════════════════════════════╗
║  CONTAINER RUNTIME MATRIX                                             ║
║                                                                       ║
║  [ OCI Runtime + Container Engine + Shim = Container Execution ]      ║
║                                                                       ║
║  "O maestro que orquestra a execução dos containers"                 ║
║                                                                       ║
╚═══════════════════════════════════════════════════════════════════════╝
```

## Arquitetura do Container Runtime 🏗️

### Camadas de Runtime
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

### Componentes do Kernel
1. **Namespaces**
   ```bash
   # Listar namespaces
   lsns
   
   # Criar novo namespace
   unshare --mount --uts --ipc --net --pid --fork --user /bin/bash
   ```

2. **Cgroups v2**
   ```bash
   # Criar cgroup
   mkdir /sys/fs/cgroup/container1
   
   # Configurar limites
   echo "100000" > /sys/fs/cgroup/container1/cpu.max
   echo "256M" > /sys/fs/cgroup/container1/memory.max
   ```

3. **Capabilities**
   ```bash
   # Listar capabilities
   capsh --print
   
   # Dropar capabilities
   capsh --drop=cap_sys_admin --
   ```

## OCI (Open Container Initiative) 📋

### Runtime Specification Detalhada
```json
{
  "ociVersion": "1.0.2",
  "root": {
    "path": "rootfs",
    "readonly": true
  },
  "process": {
    "terminal": true,
    "user": {
      "uid": 0,
      "gid": 0
    },
    "args": [
      "sh"
    ],
    "env": [
      "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
      "TERM=xterm"
    ],
    "cwd": "/",
    "capabilities": {
      "bounding": [
        "CAP_AUDIT_WRITE",
        "CAP_KILL",
        "CAP_NET_BIND_SERVICE"
      ]
    }
  },
  "mounts": [
    {
      "destination": "/proc",
      "type": "proc",
      "source": "proc"
    }
  ]
}
```

### Image Manifest Detalhado
```json
{
  "schemaVersion": 2,
  "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
  "config": {
    "mediaType": "application/vnd.docker.container.image.v1+json",
    "size": 7023,
    "digest": "sha256:b5b2b2c507a0944348e0303114d8d93aaaa081732b86451d9bce1f432a537bc7"
  },
  "layers": [
    {
      "mediaType": "application/vnd.docker.image.rootfs.diff.tar.gzip",
      "size": 32654,
      "digest": "sha256:e692418e4cbaf90ca69d05a66403747baa33ee08806650b51fab815ad7fc331f"
    }
  ]
}
```

## Container Engine: containerd 🛠️

### Arquitetura Detalhada
```ascii
┌─────────────────────────────────────────────┐
│ containerd                                  │
├─────────────────────────────────────────────┤
│ ┌─────────────┐ ┌──────────┐ ┌──────────┐  │
│ │ Image Store │ │ Metadata │ │ Snapshots│  │
│ └─────────────┘ └──────────┘ └──────────┘  │
│ ┌─────────────┐ ┌──────────┐ ┌──────────┐  │
│ │   Events    │ │  Tasks   │ │ Plugins  │  │
│ └─────────────┘ └──────────┘ └──────────┘  │
└─────────────────────────────────────────────┘
```

### API gRPC
```protobuf
service Containers {
  rpc Create(CreateContainerRequest) returns (CreateContainerResponse);
  rpc Get(GetContainerRequest) returns (GetContainerResponse);
  rpc List(ListContainersRequest) returns (ListContainersResponse);
  rpc Delete(DeleteContainerRequest) returns (DeleteContainerResponse);
  rpc Update(UpdateContainerRequest) returns (UpdateContainerResponse);
}
```

### Plugins Avançados
```ascii
┌────────────────────────────┐
│ Plugin Architecture       │
├────────────────────────────┤
│ ├── Snapshotter        │
│ ├── Content Store      │
│ ├── Diff Service      │
│ └── Metadata Store    │
└────────────────────────────┘
```

## Low-Level Runtime: runc ⚙️

### Internals do runc
```c
// libcontainer/configs/config.go
type Config struct {
    Rootfs     string   `json:"rootfs"`
    ReadonlyFs bool     `json:"readonly"`
    Namespaces []Namespace `json:"namespaces"`
    Cgroups    *Cgroup    `json:"cgroups"`
    Devices    []*Device  `json:"devices"`
    Mounts     []*Mount   `json:"mounts"`
    Hooks      Hooks      `json:"hooks"`
}
```

### Hooks do Lifecycle
```json
{
  "prestart": [
    {
      "path": "/usr/bin/fix-mounts",
      "args": ["fix-mounts", "arg1"],
      "env": [ "key1=value1" ]
    }
  ],
  "poststart": [
    {
      "path": "/usr/bin/notify-start",
      "timeout": 5
    }
  ],
  "poststop": [
    {
      "path": "/usr/bin/cleanup",
      "args": ["cleanup", "arg1"]
    }
  ]
}
```

## Container Shim v2 

### Arquitetura do Shim
```ascii
┌────────────────────────────────────┐
│ containerd                         │
├────────────────────────────────────┤
│ shim.v2.Task                      │
│ ├── Start()                      │
│ ├── Delete()                     │
│ ├── Exec()                       │
│ ├── ResizePty()                 │
│ ├── State()                      │
│ └── Kill()                       │
└────────────────────────────────────┘
```

### Implementação do Shim
```go
type Shim interface {
    StartShim(ctx context.Context, id string, opts ShimOpts) (string, error)
    Cleanup(ctx context.Context) (*Exit, error)
    Connect(ctx context.Context, onClose func()) error
}
```

## Runtime Alternatives Detalhadas 🔄

### gVisor Internals
```ascii
┌────────────────────────────┐
│ Application               │
├────────────────────────────┤
│ Sentry (User Space)       │
├────────────────────────────┤
│ Gofer (File Access)       │
├────────────────────────────┤
│ Platform (KVM/ptrace)     │
└────────────────────────────┘
```

### Kata Containers Architecture
```ascii
┌────────────────────────────┐
│ Container                 │
├────────────────────────────┤
│ Kata Runtime             │
├────────────────────────────┤
│ Lightweight VM           │
├────────────────────────────┤
│ Hypervisor              │
└────────────────────────────┘
```

## Advanced Security Features 🔒

### SELinux Policies
```bash
# Política para container
cat > container.te <<EOF
policy_module(container, 1.0)
require {
    type container_t;
    type container_file_t;
    class file { read write };
}
allow container_t container_file_t:file { read write };
EOF
```

### AppArmor Profile Detalhado
```apparmor
profile docker-default flags=(attach_disconnected,mediate_deleted) {
  network,
  capability,
  file,
  umount,

  deny @{PROC}/{*,**^[0-9*],sys/kernel/shm*} wkx,
  deny @{PROC}/sysrq-trigger rwklx,
  deny @{PROC}/mem rwklx,
  deny @{PROC}/kmem rwklx,
}
```

## Advanced Networking Features 🌐

### CNI Plugin Implementation
```go
type CNI interface {
    Setup(ctx context.Context, id string, path string, opts ...Opt) (*CNIResult, error)
    Remove(ctx context.Context, id string, path string, opts ...Opt) error
    Load(opts ...Opt) error
    Status() error
}
```

### Network Namespace Configuration
```bash
# Criar veth pair
ip link add veth0 type veth peer name veth1

# Mover para namespace
ip link set veth1 netns container1

# Configurar IP
nsenter -t $PID -n ip addr add 172.17.0.2/16 dev eth0
```

## Storage Internals 💾

### Overlay2 Driver
```ascii
┌────────────────────────────┐
│ Container Layer           │
├────────────────────────────┤
│ Upper Dir                │
├────────────────────────────┤
│ Work Dir                 │
├────────────────────────────┤
│ Merged Dir               │
├────────────────────────────┤
│ Lower Dir                │
└────────────────────────────┘
```

### Volume Driver Plugin
```go
type Driver interface {
    Create(name string, opts map[string]string) error
    Remove(name string) error
    Path(name string) string
    Mount(name string) (string, error)
    Unmount(name string) error
}
```

## Advanced Debugging Techniques 🔍

### Tracing com BPF
```bash
# Trace syscalls
bpftrace -e 'tracepoint:syscalls:sys_enter_* /pid == 1234/ { @[probe] = count(); }'

# Trace network
bpftrace -e 'tracepoint:net:netif_receive_skb { @packets = count(); }'
```

### Core Dumps Analysis
```bash
# Gerar core dump
echo "core.%e.%p" > /proc/sys/kernel/core_pattern

# Analisar com gdb
gdb `which containerd` /tmp/core.containerd.12345
```

## Performance Profiling ⚡

### CPU Profiling
```bash
# Profiling com perf
perf record -F 99 -p $PID -g -- sleep 30

# Análise
perf report --stdio
```

### Memory Analysis
```bash
# Heap profile
go tool pprof http://localhost:6060/debug/pprof/heap

# Goroutine analysis
go tool pprof http://localhost:6060/debug/pprof/goroutine
```

## Runtime Metrics & Monitoring 📊

### Prometheus Metrics
```yaml
- job_name: 'containerd'
  static_configs:
    - targets: ['localhost:9100']
  metrics_path: '/metrics'
```

### Custom Metrics
```go
var (
    containerStarts = prometheus.NewCounter(prometheus.CounterOpts{
        Name: "container_starts_total",
        Help: "Total number of container starts",
    })
)
```

## Advanced Configuration 🛠️

### Runtime Config
```toml
[plugins."io.containerd.grpc.v1.cri"]
  stream_server_address = "127.0.0.1"
  stream_server_port = "0"
  enable_selinux = false
  enable_tls_streaming = false
  
[plugins."io.containerd.grpc.v1.cri".containerd]
  snapshotter = "overlayfs"
  default_runtime_name = "runc"
```

### Custom Runtime Handler
```yaml
runtime_handler_pool:
  runtime_handlers:
    - name: "custom-handler"
      runtime_type: "io.containerd.runc.v2"
      runtime_root: "/run/containerd/custom-handler"
      privileged_without_host_devices: false
```

## Waifu Advanced Tips 🎮

> **Kernel-chan sugere:** "Namespaces e cgroups são como os poderes mágicos dos containers - use-os com sabedoria!"
> {style="tip"}

> **Security-sama alerta:** "AppArmor e SELinux são seus guardiões - nunca os desative em produção!"
> {style="warning"}

> **Debug-chan ensina:** "strace e perf são suas ferramentas de investigação - domine-as para resolver qualquer mistério!"
> {style="note"}

## Referências Avançadas 📚

### Especificações
- [OCI Runtime Spec v1.0](https://github.com/opencontainers/runtime-spec)
- [Container Network Interface Spec](https://github.com/containernetworking/cni)
- [Image Spec v1.0](https://github.com/opencontainers/image-spec)

### Documentação Técnica
- containerd Architecture
- runc Internals
- Linux Kernel Namespaces
- Control Groups v2

### Ferramentas de Debug
- runc debug
- ctr debug
- crictl debug
- containerd-debug

### Recursos de Performance
- perf-tools
- bpftrace
- systemtap
- ftrace

### Segurança
- AppArmor Profiles
- SELinux Policies
- Seccomp Filters
- Linux Capabilities

### Networking
- CNI Plugins
- Network Namespaces
- iptables
- tc (traffic control)

### Storage
- Overlay2
- Device Mapper
- BTRFS
- ZFS

## Glossário 📚

### Termos Essenciais
```ascii
┌────────────────────────────────────────────┐
│ OCI      : Open Container Initiative       │
│ CNI      : Container Network Interface     │
│ Seccomp  : Secure Computing Mode          │
│ AppArmor : Sistema MAC de segurança       │
│ SELinux  : Security-Enhanced Linux        │
│ cgroups  : Control Groups                 │
└────────────────────────────────────────────┘
```

### Componentes do Runtime
```ascii
┌────────────────────────────────────────────┐
│ containerd : Container engine de alto nível│
│ runc      : Runtime OCI de baixo nível    │
│ shim      : Processo intermediário        │
└────────────────────────────────────────────┘
```