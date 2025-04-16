# Container Basics: Primeiros Passos 📦

```ascii
╔═══════════════════════════════════════════════════════════╗
║ CONTAINER BASICS GUIDE                                    ║
║                                                          ║
║ STATUS: INICIANTE                                        ║
║ OBJETIVO: ENTENDER OS FUNDAMENTOS                        ║
║                                                          ║
╚═══════════════════════════════════════════════════════════╝
```

## Conceitos Básicos 🎯

### O que é um Container?
Um container é uma unidade padronizada de software que encapsula:
- Código da aplicação
- Bibliotecas necessárias
- Dependências do sistema
- Arquivos de configuração

```ascii
┌──────────────────────┐
│    Sua Aplicação     │
├──────────────────────┤
│    Dependências      │
├──────────────────────┤
│      Runtime         │
└──────────────────────┘
```

### Diferenças entre Container e VM
```ascii
┌─────────┐ ┌─────────┐    ┌─────────┐ ┌─────────┐
│ App 1   │ │ App 2   │    │ App 1   │ │ App 2   │
├─────────┤ ├─────────┤    ├─────────┤ ├─────────┤
│ Bins/   │ │ Bins/   │    │   OS    │ │   OS    │
│ Libs    │ │ Libs    │    ├─────────┤ ├─────────┤
├─────────┴─┴─────────┤    │  Hyper  │ │  Hyper  │
│  Docker Engine      │    │  visor  │ │  visor  │
├───────────────────┬─┤    ├─────────┴─┴─────────┤
│       OS         │ │    │      Hardware        │
└───────────────────┴─┘    └───────────────────────┘
    Containers              Máquinas Virtuais
```

## Arquitetura Básica 🏗️

### Componentes Principais
1. **Docker Daemon (dockerd)**
   - Gerencia objetos Docker
   - Manipula containers
   - Gerencia redes e volumes

2. **Docker Client (docker)**
   - Interface CLI
   - Comunica com daemon
   - Executa comandos

3. **Docker Registry**
   - Armazena imagens
   - Docker Hub (padrão)
   - Registries privados

```ascii
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│Docker Client │ ──► │Docker Daemon │ ◄─► │   Registry   │
└──────────────┘     └──────────────┘     └──────────────┘
```

## Primeiros Comandos 🛠️

### Hello World
```bash
# Seu primeiro container
docker run hello-world

# Container interativo
docker run -it ubuntu bash

# Container web básico
docker run -d -p 80:80 nginx
```

### Parâmetros Comuns do Run
```bash
# Modo detached
docker run -d nginx

# Nome personalizado
docker run --name meu-nginx nginx

# Variáveis de ambiente
docker run -e MYSQL_ROOT_PASSWORD=senha mysql

# Porta e volume
docker run -p 80:80 -v /local:/container nginx
```

## Estados do Container 🔄

### Estados Principais
1. Created (Criado)
2. Running (Em execução)
3. Paused (Pausado)
4. Stopped (Parado)
5. Deleted (Removido)

```ascii
┌──────────┐     ┌──────────┐
│ Created  │ ──► │ Running  │ ◄─┐
└──────────┘     └────┬─────┘   │
                      │          │
                      ▼          │
                ┌──────────┐     │
                │ Paused   │ ────┘
                └──────────┘
                      │
                      ▼
                ┌──────────┐
                │ Stopped  │
                └──────────┘
```

### Transições de Estado
```bash
# Criar sem iniciar
docker create nginx

# Iniciar container
docker start container_id

# Pausar container
docker pause container_id

# Despausar container
docker unpause container_id

# Reiniciar container
docker restart container_id
```

## Comandos Essenciais ⚡

### Ciclo de Vida
```bash
# Criar container
docker create nginx

# Iniciar container
docker start container_id

# Parar container
docker stop container_id

# Remover container
docker rm container_id
```

### Informações e Monitoramento
```bash
# Listar containers
docker ps
docker ps -a  # Incluir parados

# Ver logs
docker logs container_id
docker logs -f container_id  # Follow logs

# Inspecionar container
docker inspect container_id

# Estatísticas em tempo real
docker stats container_id
```

### Execução e Interação
```bash
# Executar comando
docker exec container_id ls

# Shell interativo
docker exec -it container_id bash

# Copiar arquivos
docker cp arquivo.txt container_id:/path/
docker cp container_id:/path/arquivo.txt ./
```

## Recursos Básicos 📊

### Limites de Recursos
```bash
# Limitar memória
docker run --memory=512m nginx

# Limitar CPU
docker run --cpus=2 nginx

# Limitar ambos
docker run --memory=512m --cpus=2 nginx

# Reservar memória
docker run --memory-reservation=256m nginx

# Limitar swap
docker run --memory-swap=1g nginx
```

### Monitoramento de Recursos
```bash
# Ver uso em tempo real
docker stats

# Formato personalizado
docker stats --format "table {{.Container}}\t{{.CPUPerc}}\t{{.MemUsage}}"

# Inspecionar limites
docker inspect -f '{{.HostConfig.Resources}}' container_id
```

## Networking 101 🌐

### Tipos de Rede
1. **Bridge Network (padrão)**
   - Rede isolada
   - NAT para internet
   - Comunicação entre containers

2. **Host Network**
   - Usa rede do host
   - Melhor performance
   - Menos isolamento

3. **None Network**
   - Sem conectividade
   - Máximo isolamento
   - Casos especiais

### Comandos de Rede
```bash
# Listar redes
docker network ls

# Criar rede
docker network create minha-rede

# Conectar container
docker network connect minha-rede container_id

# Desconectar container
docker network disconnect minha-rede container_id

# Inspecionar rede
docker network inspect minha-rede
```

## Storage Basics 💾

### Tipos de Storage
1. **Volumes**
   - Gerenciados pelo Docker
   - Backup mais fácil
   - Melhor performance

2. **Bind Mounts**
   - Diretório do host
   - Desenvolvimento
   - Configuração

3. **tmpfs**
   - Memória apenas
   - Dados temporários
   - Alta performance

### Comandos de Storage
```bash
# Volumes
docker volume create meu-volume
docker volume ls
docker volume inspect meu-volume
docker volume rm meu-volume

# Bind Mounts
docker run -v /host/path:/container/path nginx

# tmpfs
docker run --tmpfs /app/temp nginx
```

## Variáveis de Ambiente e Configuração 🔧

### Variáveis de Ambiente
```bash
# Definir variável
docker run -e VARIAVEL=valor nginx

# Arquivo de variáveis
docker run --env-file ./env.list nginx

# Ver variáveis
docker exec container_id env
```

### Configuração de Container
```bash
# Hostname
docker run --hostname meu-host nginx

# DNS
docker run --dns 8.8.8.8 nginx

# Adicionar hosts
docker run --add-host host:ip nginx
```

## Dicas Práticas 💡

### DO's ✅
- Use nomes descritivos
- Limite recursos
- Monitore logs
- Remova containers não utilizados
- Use health checks
- Implemente restart policies
- Documente configurações

### DON'Ts ❌
- Não armazene dados no container
- Evite containers sem limites
- Não ignore logs de erro
- Não execute como root
- Não use tags latest
- Não ignore segurança
- Não negligencie backups

## Troubleshooting Básico 🔍

### Problemas Comuns
1. Container não inicia
   ```bash
   # Verificar logs
   docker logs container_id
   
   # Verificar status
   docker inspect container_id
   ```

2. Porta em uso
   ```bash
   # Verificar portas
   docker port container_id
   
   # Listar portas em uso
   netstat -tulpn
   ```

3. Sem espaço em disco
   ```bash
   # Verificar espaço
   docker system df
   
   # Limpar recursos
   docker system prune
   ```

4. Problemas de rede
   ```bash
   # Testar rede
   docker network inspect bridge
   
   # Verificar conectividade
   docker exec container_id ping host
   ```

### Ferramentas de Debug
```bash
# Logs detalhados
docker logs --details container_id

# Eventos do sistema
docker events

# Processos do container
docker top container_id

# Alterações no filesystem
docker diff container_id
```

## Checkpoint ✅

Você agora sabe:
- [x] O que é um container
- [x] Comandos básicos
- [x] Estados do container
- [x] Gerenciamento de recursos
- [x] Networking básico
- [x] Storage básico
- [x] Troubleshooting básico
- [x] Boas práticas

## Exercícios Práticos 🎯

1. **Hello World**
   ```bash
   # Execute e entenda
   docker run hello-world
   ```

2. **Nginx Básico**
   ```bash
   # Execute servidor web
   docker run -d -p 80:80 nginx
   ```

3. **Volume Persistente**
   ```bash
   # Crie e use volume
   docker volume create data
   docker run -v data:/app nginx
   ```

## Próximos Passos 🎯

1. [Container Lifecycle](container-lifecycle.md)
2. [Container Networking](container-networking.md)
3. [Container Storage](container-storage.md)
4. [Container Security](container-security.md)

> **Basic-chan diz:** "Comece devagar, pratique muito e logo você será um mestre! 🌟"
{style="tip"}

> **Container-sensei alerta:** "A base forte é o segredo do sucesso! 🎯"
{style="note"}

> **Debug-kun lembra:** "Logs são seus melhores amigos na hora do problema! 🔍"
{style="warning"}