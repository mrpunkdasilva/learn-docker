# Lab 1: Exercícios Práticos 🏋️‍♀️

```ascii
╔═══════════════════════════════════════════════════════════╗
║ EXERCISE STATUS: TREINANDO...                             ║
║ DIFICULDADE: ★★☆☆☆                                      ║
║ TEMPO ESTIMADO: 60 minutos                               ║
║ WAIFU ASSISTANT: Exercise-chan                           ║
╚═══════════════════════════════════════════════════════════╝
```

## Preparação do Ambiente 🔧

### Requisitos
- Docker instalado e funcionando
- Acesso à internet
- Terminal ou PowerShell
- Editor de texto de sua preferência

### Verificação Inicial
```bash
# Verifique a instalação do Docker
docker --version

# Verifique o daemon
docker info

# Teste o hello-world
docker run hello-world
```

## Exercício 1: Primeiro Container 🎯

### Objetivo {id="objetivo_1"}
Criar e executar um container web básico usando nginx.

### Passos {id="passos_1"}
1. **Pull da Imagem**
   ```bash
   # Baixe a imagem oficial do nginx
   docker pull nginx:alpine
   
   # Verifique a imagem
   docker images
   ```

2. **Executar Container**
   ```bash
   # Rode o container
   docker run -d -p 8080:80 --name meu-nginx nginx:alpine
   
   # Verifique se está rodando
   docker ps
   ```

3. **Teste o Acesso**
   - Abra o navegador: http://localhost:8080
   - Ou use curl:
   ```bash
   curl http://localhost:8080
   ```

4. **Exploração Básica**
   ```bash
   # Veja informações do container
   docker inspect meu-nginx
   
   # Verifique os logs
   docker logs meu-nginx
   
   # Veja estatísticas em tempo real
   docker stats meu-nginx
   ```

## Exercício 2: Customização 🎨

### Objetivo {id="objetivo_2"}
Personalizar o container nginx com conteúdo próprio.

### Passos {id="passos_2"}
1. **Estrutura do Projeto**
   ```bash
   mkdir -p meu-site/{html,conf,logs}
   cd meu-site
   ```

2. **Configuração do Nginx**
   ```nginx
   # conf/nginx.conf
   events {
       worker_connections 1024;
   }
   
   http {
       include       /etc/nginx/mime.types;
       default_type  application/octet-stream;
   
       log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                        '$status $body_bytes_sent "$http_referer" '
                        '"$http_user_agent" "$http_x_forwarded_for"';
   
       access_log  /var/log/nginx/access.log  main;
       error_log   /var/log/nginx/error.log warn;
   
       sendfile        on;
       keepalive_timeout  65;
   
       server {
           listen       80;
           server_name  localhost;
   
           location / {
               root   /usr/share/nginx/html;
               index  index.html index.htm;
               try_files $uri $uri/ =404;
           }
   
           error_page   500 502 503 504  /50x.html;
           location = /50x.html {
               root   /usr/share/nginx/html;
           }
       }
   }
   ```

3. **Conteúdo HTML Avançado**
   ```html
   <!DOCTYPE html>
   <html lang="pt-BR">
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>Docker Lab - Nginx Custom</title>
       <style>
           :root {
               --primary-color: #2496ed;
               --secondary-color: #403f4c;
               --accent-color: #e84855;
           }
   
           body {
               font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
               margin: 0;
               padding: 0;
               background: #f5f5f5;
               color: var(--secondary-color);
           }
   
           .container {
               max-width: 800px;
               margin: 40px auto;
               padding: 20px;
           }
   
           .card {
               background: white;
               border-radius: 8px;
               padding: 20px;
               margin-bottom: 20px;
               box-shadow: 0 2px 4px rgba(0,0,0,0.1);
           }
   
           .header {
               background: var(--primary-color);
               color: white;
               padding: 20px;
               text-align: center;
           }
   
           .status {
               display: inline-block;
               padding: 8px 16px;
               background: #4CAF50;
               color: white;
               border-radius: 20px;
               font-size: 14px;
           }
   
           .docker-info {
               display: grid;
               grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
               gap: 20px;
               margin-top: 20px;
           }
   
           .info-item {
               background: #f8f9fa;
               padding: 15px;
               border-radius: 6px;
               text-align: center;
           }
   
           .footer {
               text-align: center;
               margin-top: 40px;
               padding: 20px;
               color: var(--secondary-color);
           }
       </style>
   </head>
   <body>
       <div class="header">
           <h1>🐳 Docker Lab - Nginx Custom</h1>
           <span class="status">Container Ativo</span>
       </div>
   
       <div class="container">
           <div class="card">
               <h2>Informações do Container</h2>
               <div class="docker-info">
                   <div class="info-item">
                       <h3>Container ID</h3>
                       <p>CONTAINER_ID</p>
                   </div>
                   <div class="info-item">
                       <h3>Image</h3>
                       <p>nginx:alpine</p>
                   </div>
                   <div class="info-item">
                       <h3>Port</h3>
                       <p>80:8080</p>
                   </div>
               </div>
           </div>
   
           <div class="card">
               <h2>Status do Servidor</h2>
               <ul>
                   <li>Nginx Version: <strong>Latest Alpine</strong></li>
                   <li>Server Status: <strong>Running</strong></li>
                   <li>Configuration: <strong>Custom</strong></li>
               </ul>
           </div>
       </div>
   
       <footer class="footer">
           <p>Docker Lab Exercise - Created with 💙 by Exercise-chan</p>
       </footer>
   </body>
   </html>
   ```

4. **Dockerfile Avançado**
   ```dockerfile
   # Estágio de build
   FROM nginx:alpine as builder
   
   # Instalação de ferramentas de debug
   RUN apk add --no-cache curl nano htop
   
   # Configuração do nginx
   COPY conf/nginx.conf /etc/nginx/nginx.conf
   
   # Configuração do conteúdo
   COPY html/ /usr/share/nginx/html/
   
   # Criação de diretórios necessários
   RUN mkdir -p /var/log/nginx \
       && mkdir -p /var/cache/nginx
   
   # Configuração de permissões
   RUN chown -R nginx:nginx /var/log/nginx \
       && chown -R nginx:nginx /var/cache/nginx \
       && chown -R nginx:nginx /usr/share/nginx/html
   
   # Healthcheck
   HEALTHCHECK --interval=30s --timeout=3s \
     CMD curl -f http://localhost/ || exit 1
   
   # Exposição da porta
   EXPOSE 80
   
   # Comando de inicialização
   CMD ["nginx", "-g", "daemon off;"]
   ```

5. **Build e Deploy**
   ```bash
   # Build da imagem
   docker build -t meu-site:v1 .
   
   # Execute com volumes
   docker run -d \
     --name site-custom \
     -p 8081:80 \
     -v $(pwd)/logs:/var/log/nginx \
     -v $(pwd)/html:/usr/share/nginx/html \
     meu-site:v1
   ```

## Exercício 3: Gerenciamento Avançado 🛠️

### Objetivo {id="objetivo_3"}
Explorar recursos avançados de gerenciamento de containers.

### Monitoramento Avançado
```bash
# Monitoramento detalhado
docker stats --format "table {{.Name}}\t{{.CPUPerc}}\t{{.MemUsage}}\t{{.NetIO}}"

# Histórico de eventos
docker events --filter 'container=site-custom'

# Inspeção de rede
docker network inspect bridge
```

### Backup e Restore
```bash
# Backup do container
docker commit site-custom backup-nginx:v1

# Export como tar
docker save backup-nginx:v1 > backup-nginx.tar

# Restore do backup
docker load < backup-nginx.tar
```

### Limitação de Recursos
```bash
# Execute com limites
docker run -d \
  --name site-limited \
  --cpus="1.5" \
  --memory="512m" \
  --memory-swap="1g" \
  -p 8082:80 \
  meu-site:v1
```

## Exercício 4: Networking Avançado 🌐

### Objetivo {id="objetivo_4"}
Explorar configurações avançadas de rede.

### Redes Customizadas
```bash
# Crie uma rede
docker network create --driver bridge minha-rede

# Conecte containers
docker network connect minha-rede site-custom

# Inspecione conexões
docker network inspect minha-rede
```

### DNS e Service Discovery
```bash
# Execute containers na mesma rede
docker run -d \
  --name site-1 \
  --network minha-rede \
  meu-site:v1

docker run -d \
  --name site-2 \
  --network minha-rede \
  meu-site:v1

# Teste comunicação
docker exec -it site-1 ping site-2
```

## Exercise-chan Tips 💡

> **Exercise-chan diz:** "Use `docker history meu-site:v1` para entender como sua imagem foi construída!"
{style="tip"}

> **Exercise-chan alerta:** "Monitore o uso de recursos com `docker stats` para evitar surpresas!"
{style="warning"}

> **Exercise-chan sugere:** "Experimente diferentes tags do nginx para comparar tamanhos e recursos!"
{style="note"}

## Desafios Avançados 🌟

### Desafio 1: Alta Disponibilidade
- [ ] Configure múltiplas réplicas
- [ ] Implemente health checks
- [ ] Configure restart policies
- [ ] Teste failover scenarios

### Desafio 2: Segurança
- [ ] Configure TLS/SSL
- [ ] Implemente rate limiting
- [ ] Configure access control
- [ ] Faça scanning de vulnerabilidades

### Desafio 3: CI/CD
- [ ] Crie pipeline de build
- [ ] Implemente testes automatizados
- [ ] Configure deploy automático
- [ ] Implemente versionamento

## Troubleshooting Avançado 🔍

### Debug Tools
```bash
# Análise de sistema
docker system df -v

# Debug de rede
docker network inspect bridge

# Análise de logs
docker logs --tail=100 -f site-custom | grep ERROR
```

### Common Issues Matrix

| Problema | Diagnóstico | Solução | Prevenção |
|----------|-------------|---------|-----------|
| 502 Bad Gateway | `docker logs` | Verificar nginx.conf | Health checks |
| Memory Issues | `docker stats` | Aumentar limites | Monitoramento |
| Network Timeout | `docker network inspect` | Verificar DNS | Configurar timeouts |
| Permission Denied | `ls -la` | Ajustar chmod/chown | Usar volumes |

## Checklist de Conclusão ✅

### Básico
- [ ] Container nginx rodando
- [ ] Página customizada acessível
- [ ] Logs funcionando
- [ ] Volumes configurados

### Intermediário
- [ ] Configuração nginx personalizada
- [ ] Limites de recursos definidos
- [ ] Backup configurado
- [ ] Monitoramento ativo

### Avançado
- [ ] Rede customizada configurada
- [ ] Health checks implementados
- [ ] Security hardening aplicado
- [ ] CI/CD configurado

## Próximos Passos 🎯

1. [Docker Commands](docker-commands.md)
2. [Container Basics](containers-101.md)
3. [Docker Compose](docker-compose-intro.md)
4. [Docker Security](docker-security.md)
5. [Container Orchestration](container-orchestration.md)

## Recursos Adicionais 📚

- [Nginx Official Documentation](https://nginx.org/en/docs/)
- [Docker Best Practices](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)
- [Docker Security Guide](https://docs.docker.com/engine/security/)
- [Docker Networking Guide](https://docs.docker.com/network/)
- [Docker Performance Tuning](https://docs.docker.com/config/containers/resource_constraints/)

> **Sensei's Note:** "Lembre-se: cada erro é uma oportunidade de aprendizado. Continue experimentando e documentando suas descobertas!"
{style="note"}