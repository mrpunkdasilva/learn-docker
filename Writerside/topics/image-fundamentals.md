# Fundamentos de Imagens Docker 📦

```ascii
╔══════════════════════════════════════════════════════════════════╗
║ IMAGE FUNDAMENTALS MATRIX                                        ║
║                                                                  ║
║    ┌─────────┐     ┌─────────┐     ┌─────────┐                 ║
║    │  Layer  │────▶│  Layer  │────▶│  Layer  │                 ║
║    └─────────┘     └─────────┘     └─────────┘                 ║
║         │              │               │                        ║
║    Base Image     Dependencies      App Code                    ║
║                                                                ║
╚══════════════════════════════════════════════════════════════════╝
```

## Conceitos Fundamentais 🧠

### O que é uma Imagem Docker?
Uma imagem Docker é um pacote leve, autônomo e executável que inclui tudo necessário para rodar uma aplicação:
- Código
- Runtime
- Bibliotecas
- Variáveis de ambiente
- Arquivos de configuração

### Características Principais 🎯

```ascii
╔════════════════════════════════════════════════════════════╗
║ CARACTERÍSTICAS ESSENCIAIS                                 ║
╠════════════════════════════════════════════════════════════╣
║ ✦ Imutabilidade                                           ║
║ ✦ Sistema de Camadas                                      ║
║ ✦ Compartilhamento de Recursos                            ║
║ ✦ Portabilidade                                           ║
║ ✦ Versionamento                                           ║
╚════════════════════════════════════════════════════════════╝
```

## Anatomia de uma Imagem 🔬

### Sistema de Camadas
1. **Base Layer**
   - Sistema operacional mínimo
   - Bibliotecas essenciais

2. **Camadas Intermediárias**
   - Dependências
   - Ferramentas
   - Configurações

3. **Camada de Aplicação**
   - Código fonte
   - Assets
   - Configurações específicas

## Tipos de Imagens 🏷️

### 1. Imagens Base
- Alpine
- Ubuntu
- Debian
- Scratch

### 2. Imagens de Runtime
- Node.js
- Python
- Java
- Go

### 3. Imagens de Aplicação
- Nginx
- MongoDB
- Redis
- PostgreSQL

## Nomenclatura e Tags 🏷️

```ascii
╔════════════════════════════════════════════════════════════╗
║ ANATOMIA DE UM NOME DE IMAGEM                             ║
║                                                           ║
║  registry.example.com/organization/image-name:tag         ║
║  └─── Registry ─┘ └── Owner ──┘ └─ Name ─┘ └─ Tag ─┘     ║
║                                                           ║
╚════════════════════════════════════════════════════════════╝
```

### Convenções de Tags
- `latest`: versão mais recente
- `1.0.0`: versão específica
- `stable`: versão estável
- `alpine`: baseada em Alpine Linux

## Comandos Essenciais ⌨️

```bash
# Listar imagens locais
docker images

# Baixar uma imagem
docker pull nginx:latest

# Inspecionar uma imagem
docker inspect nginx:latest

# Remover uma imagem
docker rmi nginx:latest

# Mostrar histórico de camadas
docker history nginx:latest
```

## Boas Práticas 📝

### DO's ✅
- Use tags específicas
- Minimize o tamanho das imagens
- Documente dependências
- Utilize multi-stage builds
- Mantenha imagens atualizadas

### DON'Ts ❌
- Evite `latest` em produção
- Não armazene secrets
- Não instale ferramentas desnecessárias
- Não use imagens não oficiais sem verificação
- Não ignore vulnerabilidades

## Waifu Tips 💡

> **Image-chan diz:** "Mantenha suas camadas organizadas como um bento box perfeito! 🍱"
{style="tip"}

> **Security-chan avisa:** "Sempre verifique a origem das suas imagens base! 🔍"
{style="warning"}

## Próximos Passos 🎯

1. [Anatomia de uma Imagem](image-anatomy.md)
2. [Gerenciamento de Imagens](image-management.md)
3. [Melhores Práticas](best-practices.md)

## Checkpoint ✅

Você agora entende:
- [x] O que são imagens Docker
- [x] Sistema de camadas
- [x] Tipos de imagens
- [x] Nomenclatura e tags
- [x] Comandos básicos
- [x] Boas práticas

> **Sensei's Corner:** "Uma imagem bem construída é como um kata bem executado - precisa, eficiente e elegante."
{style="note"}