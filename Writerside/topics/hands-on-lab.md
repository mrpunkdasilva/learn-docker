# Laboratório Prático: Mão na Massa! 🧪

```ascii
╔═══════════════════════════════════════════════════════════╗
║ DOCKER LAB SIMULATOR v1.0                                 ║
║                                                          ║
║ STATUS: INICIANDO SIMULAÇÃO...                           ║
║ DIFICULDADE: PROGRESSIVA                                 ║
║ OBJETIVO: DOMINAR A MATRIX DOS CONTAINERS                ║
║                                                          ║
╚═══════════════════════════════════════════════════════════╝
```

## Lab 1: Primeira Aplicação 🚀

### Objetivo
Criar uma aplicação web simples usando Docker

### Passos
1. **Criar Dockerfile**
```dockerfile
FROM python:3.9-slim
WORKDIR /app
COPY app.py requirements.txt ./
RUN pip install -r requirements.txt
CMD ["python", "app.py"]
```

2. **Criar Aplicação**
```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello():
    return "Olá, Docker!"

if __name__ == '__main__':
    app.run(host='0.0.0.0')
```

3. **Build & Run**
```bash
# Build da imagem
docker build -t minha-app:v1 .

# Executar container
docker run -p 5000:5000 minha-app:v1
```

## Lab 2: Multi-Container Setup 🎭

### Objetivo
Criar aplicação com frontend, backend e banco de dados

### Docker Compose
```yaml
version: '3.8'
services:
  frontend:
    build: ./frontend
    ports:
      - "80:80"
    depends_on:
      - backend

  backend:
    build: ./backend
    environment:
      - DB_HOST=db
    depends_on:
      - db

  db:
    image: postgres:13
    volumes:
      - pgdata:/var/lib/postgresql/data

volumes:
  pgdata:
```

### Teste de Integração
```bash
# Subir ambiente
docker-compose up -d

# Verificar logs
docker-compose logs -f

# Testar conexões
curl localhost/api/health
```

## Lab 3: Debugging & Troubleshooting 🔍

### Cenário 1: Container Crashando
```bash
# Investigar logs
docker logs container_id

# Acessar container
docker exec -it container_id sh

# Verificar recursos
docker stats container_id
```

### Cenário 2: Problemas de Rede
```bash
# Criar rede
docker network create mynet

# Inspecionar rede
docker network inspect mynet

# Testar conectividade
docker run --network mynet busybox ping service_name
```

## Lab 4: Otimização de Imagem 🎯

### Antes
```dockerfile
FROM ubuntu
RUN apt-get update
RUN apt-get install -y python3
RUN apt-get install -y python3-pip
COPY . /app
RUN pip install -r requirements.txt
CMD ["python3", "app.py"]
```

### Depois
```dockerfile
FROM python:3.9-alpine
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
CMD ["python3", "app.py"]
```

## Lab 5: Segurança & Monitoramento 🔒

### Security Scan
```bash
# Scan de vulnerabilidades
docker scan minha-app:v1

# Análise de configuração
docker bench security
```

### Monitoring Setup
```yaml
version: '3.8'
services:
  prometheus:
    image: prom/prometheus
    ports:
      - "9090:9090"

  grafana:
    image: grafana/grafana
    ports:
      - "3000:3000"
```

## Desafios Extras 🏆

### Challenge 1: High Availability
- Implementar load balancing
- Configurar health checks
- Simular failover

### Challenge 2: CI/CD Pipeline
```yaml
# .github/workflows/docker.yml
name: Docker CI

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Build
        run: docker build -t app:test .
      - name: Test
        run: docker run app:test pytest
```

## Waifu Lab Assistant Tips 💡

> **Debug-chan diz:** "Quando tudo der errado, `docker logs` é seu melhor amigo! 🔍"
{style="tip"}

> **Security-sama alerta:** "Nunca se esqueça de remover as credenciais dos logs! 🔐"
{style="warning"}

## Checklist de Conclusão ✅

### Lab 1
- [ ] Dockerfile criado
- [ ] Aplicação rodando
- [ ] Portas mapeadas

### Lab 2
- [ ] Compose configurado
- [ ] Serviços comunicando
- [ ] Volumes persistentes

### Lab 3
- [ ] Debug realizado
- [ ] Logs analisados
- [ ] Rede configurada

### Lab 4
- [ ] Imagem otimizada
- [ ] Tamanho reduzido
- [ ] Build mais rápido

### Lab 5
- [ ] Security scan
- [ ] Monitoring ativo
- [ ] Alertas configurados

## Próximos Passos 🎯

1. [Guia de Troubleshooting](troubleshooting-guide.md)
2. [Melhores Práticas](best-practices.md)
3. [Certificação Docker](docker-certified-topics.md)

> **Sensei's Note:** "A prática leva à perfeição, mas backups levam à paz de espírito."
{style="note"}