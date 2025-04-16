# Docker Compose: Orquestrando Containers 🎼

```ascii
╔════════════════════════════════════════════════════════════╗
║ DOCKER COMPOSE OVERVIEW                                    ║
║                                                           ║
║ Services │ Networks │ Volumes │ Environment │ Dependencies ║
║                                                           ║
║ Development │ Testing │ Production │ CI/CD                ║
╚════════════════════════════════════════════════════════════╝
```

## O que é Docker Compose? 🤔

Docker Compose é uma ferramenta para definir e executar aplicações Docker multi-container. Com um único arquivo YAML, você configura todos os serviços da sua aplicação e, com um único comando, cria e inicia todos os serviços.

### Anatomia do docker-compose.yml
```yaml
version: "3.8"
services:
  web:
    build: ./web
    ports:
      - "8000:8000"
    depends_on:
      - db
  db:
    image: postgres:15
    volumes:
      - db-data:/var/lib/postgresql/data
volumes:
  db-data:
```

## Conceitos Fundamentais 📚

### 1. Services
Cada container que compõe sua aplicação.
```yaml
services:
  frontend:
    image: nginx:alpine
    ports:
      - "80:80"
  
  backend:
    build: ./backend
    environment:
      - NODE_ENV=development
```

### 2. Networks
Redes que conectam seus containers.
```yaml
services:
  api:
    networks:
      - backend-network
  
networks:
  backend-network:
    driver: bridge
```

### 3. Volumes
Armazenamento persistente para seus dados.
```yaml
services:
  db:
    volumes:
      - db_data:/var/lib/mysql
      
volumes:
  db_data:
```

## Exemplo Completo: Stack Web 🌐

```yaml
version: "3.8"

services:
  frontend:
    build: 
      context: ./frontend
      dockerfile: Dockerfile.dev
    ports:
      - "3000:3000"
    volumes:
      - ./frontend:/app
      - /app/node_modules
    environment:
      - REACT_APP_API_URL=http://api:5000
    depends_on:
      - api

  api:
    build: ./backend
    ports:
      - "5000:5000"
    environment:
      - DATABASE_URL=postgres://user:pass@db:5432/dbname
    depends_on:
      - db

  db:
    image: postgres:15-alpine
    environment:
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=pass
      - POSTGRES_DB=dbname
    volumes:
      - pgdata:/var/lib/postgresql/data

  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"

volumes:
  pgdata:

networks:
  default:
    driver: bridge
```

## Comandos Essenciais 🛠️

```bash
# Iniciar todos os serviços
docker-compose up

# Modo detached (background)
docker-compose up -d

# Parar todos os serviços
docker-compose down

# Reconstruir imagens
docker-compose build

# Logs
docker-compose logs -f

# Status dos serviços
docker-compose ps
```

## Waifu Docker Compose Tips 💡

> **Compose-chan diz:** "Mantenha seus serviços organizados e bem documentados! 📝"
{style="tip"}

> **Network-chan alerta:** "Não esqueça de configurar suas redes corretamente! 🌐"
{style="warning"}

> **Volume-chan lembra:** "Dados importantes precisam de volumes persistentes! 💾"
{style="note"}

## Padrões Comuns 🎯

### 1. Development Environment
```yaml
services:
  app:
    build: .
    volumes:
      - .:/app
    command: npm run dev
```

### 2. Testing Setup
```yaml
services:
  test:
    build:
      context: .
      target: test
    command: npm test
```

### 3. Production Configuration
```yaml
services:
  app:
    image: myapp:latest
    restart: always
    environment:
      - NODE_ENV=production
```

## Boas Práticas ✨

1. **Versionamento**
   - Sempre especifique a versão do Compose
   - Use tags específicas para imagens

2. **Organização**
   - Agrupe serviços relacionados
   - Use nomes descritivos
   - Documente variáveis de ambiente

3. **Segurança**
   - Não comite secrets
   - Use `.env` files
   - Limite exposição de portas

4. **Performance**
   - Use build cache
   - Otimize dependências
   - Configure healthchecks

## Checkpoint ✅

Você agora domina:
- [x] Estrutura básica do docker-compose.yml
- [x] Configuração de serviços
- [x] Redes e volumes
- [x] Variáveis de ambiente
- [x] Comandos essenciais
- [x] Padrões comuns
- [x] Boas práticas

## Próximos Passos 🎯

1. [Docker Compose File](docker-compose-file.md) - Referência completa
2. [Docker Compose Commands](docker-compose-commands.md) - Comandos avançados
3. [Container Operations](container-operations.md) - Operações práticas

## Recursos Adicionais 📚

- [Docker Compose Documentation](https://docs.docker.com/compose/)
- [Compose File Reference](https://docs.docker.com/compose/compose-file/)
- [Awesome Compose Examples](https://github.com/docker/awesome-compose)

> **Sensei's Note:** "Docker Compose é como um maestro - coordena todos os instrumentos (containers) para criar uma sinfonia perfeita! 🎵"
{style="note"}