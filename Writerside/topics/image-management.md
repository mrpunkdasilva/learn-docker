# Gerenciamento de Imagens Docker 🎮

```ascii
╔══════════════════════════════════════════════════════════════════╗
║ IMAGE MANAGEMENT MATRIX                                          ║
║                                                                  ║
║    ┌─────────┐     ┌─────────┐     ┌─────────┐                 ║
║    │  Pull   │────▶│  Tag    │────▶│  Push   │                 ║
║    └─────────┘     └─────────┘     └─────────┘                 ║
║         ↑              │               │                        ║
║         └──────────────┴───────────────┘                       ║
║                                                                ║
╚══════════════════════════════════════════════════════════════════╝
```

## Comandos Essenciais 🛠️

### Gerenciamento Básico
```bash
# Listar imagens
docker images

# Baixar imagem
docker pull nginx:latest

# Remover imagem
docker rmi nginx:latest

# Remover todas imagens não utilizadas
docker image prune
```

### Tagging e Versionamento
```bash
# Criar tag
docker tag nginx:latest meu-registry.com/nginx:v1.0

# Remover tag
docker rmi meu-registry.com/nginx:v1.0

# Listar tags de uma imagem
docker image inspect nginx --format='{{.RepoTags}}'
```

## Sistema de Tags 🏷️

```ascii
╔════════════════════════════════════════════════════════════╗
║ TAG CONVENTIONS                                            ║
║                                                           ║
║  registry.example.com/org/app:1.0.0-alpine               ║
║  └─── Registry ─┘ └─ Name ─┘ └─ Tag ─┘ └─ Variant ─┘    ║
║                                                           ║
╚════════════════════════════════════════════════════════════╝
```

### Convenções Comuns
- `latest`: versão mais recente
- `1.0.0`: versão semântica
- `stable`: versão estável
- `dev`: versão desenvolvimento
- `alpine`: variante específica

## Registries e Distribuição 🌐

### Docker Hub
```bash
# Login no Docker Hub
docker login

# Push para Docker Hub
docker push username/app:tag

# Pull do Docker Hub
docker pull username/app:tag
```

### Registry Privado
```bash
# Login em registry privado
docker login meu-registry.com

# Push para registry privado
docker push meu-registry.com/app:tag

# Pull de registry privado
docker pull meu-registry.com/app:tag
```

## Otimização e Limpeza 🧹

### Limpeza de Sistema
```bash
# Remover imagens não utilizadas
docker image prune -a

# Remover imagens dangling
docker image prune

# Limpar todo o sistema
docker system prune
```

### Otimização de Espaço
```bash
# Verificar uso de espaço
docker system df

# Análise detalhada
docker system df -v
```

## Boas Práticas ✨

### DO's ✅
- Use tags específicas em produção
- Mantenha um registro de versões
- Documente variantes de imagens
- Implemente política de retenção
- Automatize o processo de build/push

### DON'Ts ❌
- Evite usar `latest` em produção
- Não acumule imagens antigas
- Não armazene dados sensíveis
- Não ignore vulnerabilidades
- Não deixe imagens sem tag

## Automação e CI/CD 🤖

### GitHub Actions Example
```yaml
steps:
  - uses: actions/checkout@v2
  
  - name: Login to Registry
    run: docker login -u ${{ secrets.DOCKER_USER }} -p ${{ secrets.DOCKER_TOKEN }}
    
  - name: Build and Push
    run: |
      docker build -t app:${{ github.sha }} .
      docker push app:${{ github.sha }}
```

## Waifu Tips 💡

> **Registry-chan diz:** "Mantenha suas tags organizadas como uma coleção de mangás! 📚"
{style="tip"}

> **Space-chan avisa:** "Não deixe imagens antigas ocuparem espaço! Faça limpeza regularmente! 🧹"
{style="warning"}

## Troubleshooting 🔧

### Problemas Comuns

1. **Pull Fails**
   ```bash
   # Verificar conectividade
   docker info
   
   # Limpar cache DNS
   docker system prune --volumes
   ```

2. **Push Errors**
   ```bash
   # Verificar autenticação
   docker login
   
   # Verificar permissões
   docker push app:tag
   ```

## Próximos Passos 🎯

1. [Operações com Containers](container-operations.md)
2. [Melhores Práticas](best-practices.md)
3. [Laboratório Prático](hands-on-lab.md)

> **Sensei's Note:** "Um bom gerenciamento de imagens é como manter um dojo organizado - cada coisa em seu lugar."
{style="note"}