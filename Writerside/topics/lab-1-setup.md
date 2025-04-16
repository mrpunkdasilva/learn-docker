# Lab 1: Configuração do Ambiente 🛠️

```ascii
╔═══════════════════════════════════════════════════════════╗
║ SETUP STATUS: CONFIGURANDO...                             ║
║ DIFICULDADE: ★☆☆☆☆                                      ║
║ TEMPO ESTIMADO: 20 minutos                               ║
║ WAIFU ASSISTANT: Setup-chan                              ║
╚═══════════════════════════════════════════════════════════╝
```

## Verificação do Sistema 🔍

### Requisitos Mínimos
- CPU: 2 cores
- RAM: 4GB
- Espaço em Disco: 20GB
- Sistema Operacional: Windows 10+, macOS 10.15+, ou Linux (kernel 3.10+)

### Verificação de Compatibilidade
```bash
# Windows - PowerShell
systeminfo | findstr /B /C:"OS Name" /C:"OS Version" /C:"System Type"

# macOS - Terminal
system_profiler SPHardwareDataType | grep "Processor\|Memory"

# Linux - Terminal
uname -a
free -h
nproc
```

## Instalação do Docker 📦

### Windows
1. **Docker Desktop**
   ```bash
   # Verifique se o WSL2 está instalado
   wsl --version
   
   # Instale o Docker Desktop
   winget install Docker.DockerDesktop
   ```

### macOS
1. **Docker Desktop**
   ```bash
   # Via Homebrew
   brew install --cask docker
   
   # Inicie o Docker Desktop
   open /Applications/Docker.app
   ```

### Linux
1. **Docker Engine**
   ```bash
   # Ubuntu/Debian
   curl -fsSL https://get.docker.com -o get-docker.sh
   sudo sh get-docker.sh
   
   # Configure permissões
   sudo usermod -aG docker $USER
   ```

## Verificação da Instalação ✅

### Teste Básico
```bash
# Verifique a versão
docker version

# Execute container de teste
docker run hello-world
```

### Verificação Detalhada
```bash
# Info do sistema Docker
docker info

# Teste de rede
docker run --rm alpine ping -c 4 google.com

# Teste de disco
docker run --rm alpine dd if=/dev/zero of=/dev/null bs=1M count=1000
```

## Configuração do Projeto 📁

### Estrutura de Diretórios
```bash
# Crie a estrutura do projeto
mkdir -p lab-1/{app,config,scripts}
cd lab-1

# Crie arquivos iniciais
touch app/index.html
touch app/style.css
touch Dockerfile
touch docker-compose.yml
```

### Arquivos Base

#### 1. index.html
```html
<!DOCTYPE html>
<html>
<head>
    <title>Docker Lab 1</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <h1>Hello Docker!</h1>
</body>
</html>
```

#### 2. style.css
```css
body {
    font-family: Arial, sans-serif;
    text-align: center;
    margin-top: 50px;
}
```

#### 3. Dockerfile
```dockerfile
FROM nginx:alpine
COPY app/ /usr/share/nginx/html/
```

#### 4. docker-compose.yml
```yaml
version: '3.8'
services:
  web:
    build: .
    ports:
      - "8080:80"
```

## Configuração do Ambiente de Desenvolvimento 💻

### Visual Studio Code
1. **Extensões Recomendadas**
   - Docker
   - Remote - Containers
   - YAML
   - Dockerfile

### Terminal
```bash
# Configure aliases úteis
echo 'alias d="docker"' >> ~/.bashrc
echo 'alias dc="docker-compose"' >> ~/.bashrc
source ~/.bashrc
```

## Verificação Final 🎯

### Checklist de Setup
- [ ] Docker instalado
- [ ] Hello World executado
- [ ] Projeto estruturado
- [ ] Arquivos base criados
- [ ] IDE configurado

### Teste de Ambiente
```bash
# Build do projeto
docker build -t lab1 .

# Execute o container
docker run -d -p 8080:80 lab1

# Verifique o status
docker ps
```

## Setup-chan Tips 💡

> **Setup-chan diz:** "Sempre verifique as permissões do Docker no Linux! Se não conseguir executar comandos, adicione seu usuário ao grupo docker!"
{style="tip"}

> **Setup-chan alerta:** "No Windows, certifique-se que o WSL2 está atualizado e configurado corretamente!"
{style="warning"}

## Troubleshooting de Setup 🔧

### Problemas Comuns

#### 1. Docker não inicia
```bash
# Windows
net stop com.docker.service
net start com.docker.service

# Linux
sudo systemctl restart docker
```

#### 2. Permissões negadas
```bash
# Linux
sudo groupadd docker
sudo usermod -aG docker $USER
newgrp docker
```

#### 3. Portas em uso
```bash
# Verifique portas em uso
netstat -tulpn | grep LISTEN

# Windows
netstat -ano | findstr :80
```

## Próximos Passos 🎯

1. [Lab 1 Exercise](lab-1-exercise.md)
2. [Docker Commands](docker-commands.md)
3. [Container Basics](containers-101.md)

## Recursos de Setup 📚

- [Docker Installation Guide](https://docs.docker.com/get-docker/)
- [WSL2 Installation](https://docs.microsoft.com/windows/wsl/install)
- [Docker Desktop Settings](https://docs.docker.com/desktop/)

> **Sensei's Note:** "Um ambiente bem configurado é metade do caminho para o sucesso!"
{style="note"}