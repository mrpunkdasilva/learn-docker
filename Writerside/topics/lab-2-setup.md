# Lab 2: Configuração do Ambiente de Desenvolvimento 🛠️

```ascii
╔═══════════════════════════════════════════════════════════╗
║ SETUP STATUS: CONFIGURANDO...                             ║
║ DIFICULDADE: ★★★☆☆                                      ║
║ TEMPO ESTIMADO: 45 minutos                               ║
║ WAIFU ASSISTANT: Setup-chan                              ║
╚═══════════════════════════════════════════════════════════╝
```

## Verificação do Ambiente 🔍

### Requisitos Mínimos
- Docker Engine 20.10+
- Docker Compose v2.0+
- 8GB RAM
- 50GB Espaço em Disco
- Git instalado

### Verificação de Dependências
```bash
# Verifique versões
docker --version
docker compose version
git --version

# Verifique recursos disponíveis
docker info
```

## Estrutura do Projeto 📁

### Setup Inicial
```bash
# Clone o repositório base
git clone https://github.com/seu-usuario/docker-lab-2
cd docker-lab-2

# Crie a estrutura de diretórios
mkdir -p {frontend,backend,database,config}
```

### Arquivos Base

#### 1. docker-compose.yml
```yaml
version: '3.8'
services:
  frontend:
    build:
      context: ./frontend
      dockerfile: Dockerfile
      target: development
    volumes:
      - ./frontend:/app
      - /app/node_modules
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=development
    depends_on:
      - backend

  backend:
    build:
      context: ./backend
      dockerfile: Dockerfile
      target: development
    volumes:
      - ./backend:/app
      - /app/__pycache__
    ports:
      - "5000:5000"
    environment:
      - FLASK_ENV=development
      - DB_HOST=db
    depends_on:
      - db

  db:
    image: postgres:13-alpine
    volumes:
      - pgdata:/var/lib/postgresql/data
      - ./database/init.sql:/docker-entrypoint-initdb.d/init.sql
    environment:
      - POSTGRES_PASSWORD=devpassword
      - POSTGRES_DB=devdb

volumes:
  pgdata:
```

#### 2. frontend/Dockerfile
```dockerfile
# Estágio de desenvolvimento
FROM node:18-alpine as development
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
CMD ["npm", "run", "dev"]

# Estágio de produção
FROM node:18-alpine as production
WORKDIR /app
COPY package*.json ./
RUN npm install --production
COPY . .
RUN npm run build
CMD ["npm", "start"]
```

#### 3. backend/Dockerfile
```dockerfile
# Estágio de desenvolvimento
FROM python:3.9-slim as development
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
CMD ["flask", "run", "--host=0.0.0.0", "--debug"]

# Estágio de produção
FROM python:3.9-slim as production
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
CMD ["gunicorn", "app:app"]
```

## Configuração do Ambiente 🔧

### 1. Variáveis de Ambiente
```bash
# Crie arquivo .env na raiz
cat > .env << EOL
NODE_ENV=development
FLASK_ENV=development
DB_HOST=db
DB_PORT=5432
DB_NAME=devdb
DB_USER=postgres
DB_PASS=devpassword
EOL
```

### 2. Networks
```bash
# Crie network para desenvolvimento
docker network create dev-network

# Verifique a criação
docker network ls
```

### 3. Volumes
```bash
# Crie volumes necessários
docker volume create pgdata

# Verifique volumes
docker volume ls
```

## Setup de Desenvolvimento 💻

### Visual Studio Code
1. **Extensões Recomendadas**
   - Docker
   - Remote Containers
   - Python
   - Node.js
   - ESLint
   - Prettier

### Hot Reload
```bash
# Frontend (package.json)
{
  "scripts": {
    "dev": "vite --host"
  }
}

# Backend (requirements.txt)
flask-debug==0.4.3
python-dotenv==0.19.0
```

## Verificação do Setup ✅

### Teste de Integração
```bash
# Build dos containers
docker compose build

# Inicie os serviços
docker compose up -d

# Verifique os logs
docker compose logs -f
```

### Health Check
```bash
# Verifique status dos containers
docker compose ps

# Teste conexões
curl http://localhost:3000
curl http://localhost:5000/health
```

## Setup-chan Tips 💡

> **Setup-chan diz:** "Use `.dockerignore` para excluir arquivos desnecessários e melhorar o build! 📝"
{style="tip"}

> **Setup-chan alerta:** "Nunca exponha variáveis sensíveis nos Dockerfiles! Use `.env` ou secrets! 🔒"
{style="warning"}

## Troubleshooting 🔍

### Problemas Comuns

#### 1. Permissões de Volume
```bash
# Corrija permissões
sudo chown -R $USER:$USER .

# Verifique montagem
docker compose exec frontend ls -la /app
```

#### 2. Conexão com Database
```bash
# Teste conexão
docker compose exec backend python -c "import psycopg2; conn=psycopg2.connect(host='db', dbname='devdb', user='postgres', password='devpassword')"
```

## Próximos Passos 🎯

1. [Lab 2 Exercise](lab-2-exercise.md)
2. [Docker Compose Deep Dive](docker-compose-deep-dive.md)
3. [Development Best Practices](development-best-practices.md)

## Recursos Adicionais 📚

- [Docker Compose File Reference](https://docs.docker.com/compose/compose-file/)
- [Development with Docker](https://docs.docker.com/develop/)
- [Multi-stage Builds](https://docs.docker.com/build/building/multi-stage/)

> **Sensei's Note:** "Um ambiente bem configurado é a base para um desenvolvimento produtivo! 🎯"
{style="note"}