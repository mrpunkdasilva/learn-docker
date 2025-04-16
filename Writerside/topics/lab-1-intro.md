# Lab 1: Primeiros Passos com Docker 🚀

```ascii
╔═══════════════════════════════════════════════════════════╗
║ LAB STATUS: INICIANDO...                                  ║
║ DIFICULDADE: ★☆☆☆☆                                      ║
║ TEMPO ESTIMADO: 45 minutos                               ║
║ WAIFU ASSISTANT: Debug-chan                              ║
╚═══════════════════════════════════════════════════════════╝
```

## Objetivo do Lab 🎯

Neste laboratório inicial, você vai:
1. Criar seu primeiro container
2. Entender comandos básicos
3. Manipular imagens Docker
4. Interagir com containers

## Pré-requisitos 📝

- [x] Docker instalado
- [x] Terminal/Command Prompt
- [x] Editor de texto
- [x] Conexão com internet

## Ambiente de Desenvolvimento 🛠️

```bash
# Verifique a instalação do Docker
docker --version

# Teste o Docker
docker run hello-world
```

## Estrutura do Projeto 📁

```
lab-1/
├── app/
│   ├── index.html
│   └── style.css
├── Dockerfile
└── README.md
```

## Exercícios 📚

### 1. Hello Docker! 👋
```bash
# Pull da imagem nginx
docker pull nginx:alpine

# Rode o container
docker run -d -p 8080:80 nginx:alpine

# Verifique se está rodando
docker ps
```

### 2. Customização Básica 🎨
```dockerfile
FROM nginx:alpine
COPY app/ /usr/share/nginx/html/
```

### 3. Comandos Essenciais ⌨️
```bash
# Listar containers
docker ps

# Logs do container
docker logs [container-id]

# Entrar no container
docker exec -it [container-id] sh

# Parar container
docker stop [container-id]
```

## Desafios Extras 🌟

### Nível 1: Explorador 🔍
- [ ] Modifique o conteúdo do nginx
- [ ] Exponha em porta diferente
- [ ] Visualize os logs

### Nível 2: Aventureiro ⚔️
- [ ] Adicione variáveis de ambiente
- [ ] Configure um volume
- [ ] Crie um health check

### Nível 3: Mestre 👑
- [ ] Otimize o tamanho da imagem
- [ ] Implemente multi-stage build
- [ ] Configure network customizada

## Debug-chan Tips 💡

> **Debug-chan diz:** "Lembre-se de verificar os logs quando algo der errado! Use `docker logs [container-id]` para ver o que está acontecendo!"
{style="tip"}

> **Debug-chan alerta:** "Cuidado com as portas! Certifique-se que a porta que você quer usar não está sendo usada por outro processo!"
{style="warning"}

## Troubleshooting Comum 🔧

### Problema: Container não inicia
```bash
# Verifique os logs
docker logs [container-id]

# Verifique o status
docker inspect [container-id]
```

### Problema: Não consegue acessar a aplicação
```bash
# Verifique as portas
docker port [container-id]

# Liste todas as networks
docker network ls
```

## Checklist de Conclusão ✅

### Básico
- [ ] Docker instalado e funcionando
- [ ] Hello World executado
- [ ] Nginx container rodando
- [ ] Portas mapeadas corretamente

### Intermediário
- [ ] Dockerfile criado
- [ ] Imagem customizada buildada
- [ ] Container customizado rodando
- [ ] Logs verificados

### Avançado
- [ ] Volume configurado
- [ ] Network personalizada
- [ ] Health check implementado
- [ ] Container otimizado

## Próximos Passos 🎯

1. [Lab 1 Setup](lab-1-setup.md)
2. [Lab 1 Exercise](lab-1-exercise.md)
3. [Docker Commands](docker-commands.md)

## Recursos Adicionais 📚

- [Docker Hub](https://hub.docker.com)
- [Docker Docs](https://docs.docker.com)
- [Nginx Docs](https://nginx.org/en/docs/)

> **Sensei's Note:** "O primeiro container é como o primeiro programa 'Hello World' - parece simples, mas é o início de uma grande jornada!"
{style="note"}