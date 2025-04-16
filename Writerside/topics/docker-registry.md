# Docker Registry: Seu Repositório Privado 📦

```ascii
╔═══════════════════════════════════════════════════════════╗
║  DOCKER REGISTRY: PRIVATE IMAGE REPOSITORY                ║
║                                                          ║
║  "Seu próprio cantinho seguro para guardar imagens"      ║
╚═══════════════════════════════════════════════════════════╝
```

## Fundamentos do Registry 📚

### O que é um Registry?
- Serviço de armazenamento e distribuição
- Repositório de imagens Docker
- Sistema de versionamento
- Controle de acesso

### Tipos de Registry
1. **Docker Hub** (Público)
2. **Registry Privado**
3. **Cloud Providers**
   - Amazon ECR
   - Google Container Registry
   - Azure Container Registry

## Implantação do Registry 🚀

### Registry Local
```bash
# Iniciar registry local
docker run -d \
  -p 5000:5000 \
  --name registry \
  registry:2

# Testar registry
curl http://localhost:5000/v2/_catalog
```

### Registry com TLS
```bash
# Gerar certificados
openssl req -newkey rsa:4096 -nodes -sha256 \
  -keyout certs/domain.key -x509 -days 365 \
  -out certs/domain.crt

# Iniciar registry seguro
docker run -d \
  -p 5000:5000 \
  --name registry \
  -v $(pwd)/certs:/certs \
  -e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/domain.crt \
  -e REGISTRY_HTTP_TLS_KEY=/certs/domain.key \
  registry:2
```

## Operações Básicas 🛠️

### Push e Pull
```bash
# Tag local
docker tag nginx:latest localhost:5000/my-nginx

# Push para registry local
docker push localhost:5000/my-nginx

# Pull do registry local
docker pull localhost:5000/my-nginx
```

### Autenticação
```bash
# Configurar autenticação básica
docker run -d \
  -p 5000:5000 \
  --name registry \
  -v $(pwd)/auth:/auth \
  -e "REGISTRY_AUTH=htpasswd" \
  -e "REGISTRY_AUTH_HTPASSWD_REALM=Registry Realm" \
  -e "REGISTRY_AUTH_HTPASSWD_PATH=/auth/htpasswd" \
  registry:2

# Login no registry
docker login localhost:5000
```

## Configuração Avançada ⚙️

### Storage Backends
```yaml
storage:
  filesystem:
    rootdirectory: /var/lib/registry
  s3:
    accesskey: awsaccesskey
    secretkey: awssecretkey
    region: us-east-1
    bucket: docker-registry
```

### Garbage Collection
```bash
# Executar garbage collection
docker exec registry bin/registry garbage-collect \
  /etc/docker/registry/config.yml
```

## Alta Disponibilidade 🌟

### Registry em Cluster
```yaml
version: '3'
services:
  registry1:
    image: registry:2
    ports:
      - "5000:5000"
    environment:
      REGISTRY_HTTP_TLS_CERTIFICATE: /certs/domain.crt
      REGISTRY_HTTP_TLS_KEY: /certs/domain.key
    volumes:
      - registry-data:/var/lib/registry
      - ./certs:/certs
```

### Load Balancing
```nginx
upstream docker-registry {
  server registry1:5000;
  server registry2:5000;
}

server {
  listen 443 ssl;
  server_name registry.example.com;

  ssl_certificate /etc/nginx/certs/domain.crt;
  ssl_certificate_key /etc/nginx/certs/domain.key;

  location / {
    proxy_pass http://docker-registry;
  }
}
```

## Segurança e Políticas 🔒

### Image Signing
```bash
# Habilitar DCT (Docker Content Trust)
export DOCKER_CONTENT_TRUST=1

# Assinar imagem
docker trust sign myregistry.com/app:1.0
```

### Access Control
```yaml
auth:
  token:
    realm: https://auth.example.com/token
    service: container_registry
    issuer: auth_token_issuer
```

## Monitoramento 📊

### Métricas Básicas
```bash
# Verificar status
curl -X GET http://localhost:5000/debug/vars

# Monitorar espaço
du -sh /var/lib/registry
```

### Prometheus Integration
```yaml
metrics:
  enabled: true
  address: localhost:5001
```

## Troubleshooting 🔍

### Logs e Debug
```bash
# Verificar logs
docker logs registry

# Aumentar verbosidade
docker run -d \
  -e REGISTRY_LOG_LEVEL=debug \
  registry:2
```

### Problemas Comuns
1. **Certificados Inválidos**
   ```bash
   # Adicionar certificado ao sistema
   cp domain.crt /usr/local/share/ca-certificates/
   update-ca-certificates
   ```

2. **Problemas de Storage**
   ```bash
   # Verificar permissões
   chown -R 100:100 /var/lib/registry
   ```

## Registry UI 🖥️

### Docker Registry UI
```bash
# Iniciar interface web
docker run -d \
  -p 8080:8080 \
  --name registry-ui \
  -e REGISTRY_URL=http://registry:5000 \
  joxit/docker-registry-ui
```

## CI/CD Integration 🔄

### GitHub Actions
```yaml
jobs:
  push:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      
      - name: Login to Registry
        run: docker login -u ${{ secrets.REGISTRY_USER }} -p ${{ secrets.REGISTRY_PASS }}
        
      - name: Build and Push
        run: |
          docker build -t registry.example.com/app:${{ github.sha }} .
          docker push registry.example.com/app:${{ github.sha }}
```

## Waifu Registry Tips 💫

> **Registry-chan diz:** "Mantenha suas imagens organizadas como uma coleção de mangás!"
{style="tip"}

> **Storage-sama alerta:** "Não se esqueça do garbage collection, ou seu espaço vai sumir mais rápido que um pocky!"
{style="warning"}

## Checklist de Produção ✅

1. [x] TLS configurado
2. [x] Autenticação habilitada
3. [x] Storage backend redundante
4. [x] Monitoramento ativo
5. [x] Backup automatizado
6. [x] Políticas de retenção
7. [x] Rate limiting configurado

## Recursos Adicionais 📚

### Documentação
- [Docker Registry](https://docs.docker.com/registry/)
- [Distribution](https://github.com/distribution/distribution)
- [Registry Configuration](https://docs.docker.com/registry/configuration/)

### Ferramentas
- [Registry UI](https://github.com/Joxit/docker-registry-ui)
- [Registry CLI](https://github.com/genuinetools/reg)
- [Distribution Tools](https://github.com/distribution/distribution/tree/main/cmd/registry)

> **Registry Master's Note:** "Um registry bem configurado é como um bonsai - requer cuidado constante e podas regulares."
{style="note"}