# Lab 2: Desenvolvimento com Docker 🚀

```ascii
╔═══════════════════════════════════════════════════════════╗
║ LAB STATUS: INICIANDO...                                  ║
║ DIFICULDADE: ★★★☆☆                                        ║
║ TEMPO ESTIMADO: 90 minutos                               ║
║ WAIFU ASSISTANT: DevOps-chan                             ║
╚═══════════════════════════════════════════════════════════╝
```

## Objetivo do Lab 🎯

Neste laboratório de desenvolvimento, você vai:
1. Criar um ambiente de desenvolvimento completo
2. Trabalhar com múltiplos containers
3. Implementar Docker Compose
4. Desenvolver uma aplicação multi-container

## Pré-requisitos 📝

- [x] Lab 1 completado
- [x] Docker Compose instalado
- [x] Git instalado
- [x] Editor de código
- [x] Conhecimento básico de web development

## Ambiente de Desenvolvimento 🛠️

```bash
# Verifique a instalação do Docker Compose
docker compose version

# Clone o repositório do projeto
git clone https://github.com/seu-usuario/docker-lab-2
cd docker-lab-2
```

## Estrutura do Projeto 📁

```
lab-2/
├── frontend/
│   ├── src/
│   ├── package.json
│   └── Dockerfile
├── backend/
│   ├── src/
│   ├── requirements.txt
│   └── Dockerfile
├── database/
│   └── init.sql
├── docker-compose.yml
└── README.md
```

## Exercícios 📚

### 1. Setup do Ambiente 🏗️
```yaml
# docker-compose.yml básico
version: '3.8'
services:
  frontend:
    build: ./frontend
    ports:
      - "3000:3000"
  
  backend:
    build: ./backend
    ports:
      - "5000:5000"
  
  db:
    image: postgres:13-alpine
    environment:
      POSTGRES_PASSWORD: example
```

### 2. Desenvolvimento Frontend 🎨
```dockerfile
# frontend/Dockerfile
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
CMD ["npm", "start"]
```

### 3. Desenvolvimento Backend 🔧
```dockerfile
# backend/Dockerfile
FROM python:3.9-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
CMD ["python", "src/app.py"]
```

## Desafios Extras 🌟

### Nível 1: Developer 💻
- [ ] Implementar hot-reload
- [ ] Configurar volumes de desenvolvimento
- [ ] Adicionar linting e testes

### Nível 2: Architect 🏛️
- [ ] Implementar cache de build
- [ ] Otimizar Dockerfiles
- [ ] Configurar networks isoladas

### Nível 3: DevOps 🔄
- [ ] Setup CI/CD básico
- [ ] Implementar health checks
- [ ] Configurar logging centralizado

## DevOps-chan Tips 💡

> **DevOps-chan diz:** "Use volumes para desenvolvimento! Não precisa rebuildar a imagem a cada alteração no código! 🔄"
{style="tip"}

> **DevOps-chan alerta:** "Cuidado com as dependências entre serviços! Use `depends_on` e health checks! 🏥"
{style="warning"}

## Troubleshooting Comum 🔧

### Problema: Serviços não se comunicam
```bash
# Verifique as networks
docker network ls

# Inspecione os containers
docker inspect [container-id]
```

### Problema: Volumes não sincronizam
```bash
# Verifique os volumes
docker volume ls

# Verifique os binds
docker compose ps
```

## Checklist de Conclusão ✅

### Ambiente
- [ ] Docker Compose configurado
- [ ] Serviços rodando
- [ ] Networks configuradas
- [ ] Volumes montados

### Desenvolvimento
- [ ] Hot-reload funcionando
- [ ] Logs centralizados
- [ ] Testes implementados
- [ ] CI/CD básico configurado

### Performance
- [ ] Build otimizado
- [ ] Cache configurado
- [ ] Resources limitados
- [ ] Monitoring básico

## Próximos Passos 🎯

1. [Lab 2 Setup](lab-2-setup.md)
2. [Lab 2 Exercise](lab-2-exercise.md)
3. [Docker Compose Deep Dive](docker-compose-deep-dive.md)

## Recursos Adicionais 📚

- [Docker Compose Docs](https://docs.docker.com/compose/)
- [Development Best Practices](https://docs.docker.com/develop/dev-best-practices/)
- [Docker in Development](https://docs.docker.com/develop/)

> **Sensei's Note:** "O verdadeiro poder do Docker se revela quando você domina o desenvolvimento multi-container! 🎯"
{style="note"}