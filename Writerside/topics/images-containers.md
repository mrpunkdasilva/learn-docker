# Images vs Containers: O Dual Boot do Futuro 🌌

```ascii
╔══════════════════════════════════════════════════════════════════╗
║ IMAGES VS CONTAINERS MATRIX                                       ║
║                                                                  ║
║    ┌─────────┐                        ┌──────────┐              ║
║    │ Images  │──── docker run ───────▶│Containers│              ║
║    └─────────┘                        └──────────┘              ║
║    Blueprint                           Running Instance          ║
║                                                                 ║
╚══════════════════════════════════════════════════════════════════╝
```

## O que você vai aprender 🎯

Neste módulo, você mergulhará no universo das imagens e containers Docker, entendendo:

- A diferença fundamental entre imagens e containers
- Como imagens funcionam como blueprints para containers
- O ciclo de vida de containers
- Gerenciamento básico de imagens e containers
- Melhores práticas e padrões comuns

## A Dualidade Fundamental 🧬

### Imagens: O Blueprint Imutável
Imagens Docker são como plantas arquitetônicas digitais - templates read-only que contêm:
- Sistema operacional base
- Dependências
- Configurações
- Código da aplicação

### Containers: A Instância Viva
Containers são instâncias em execução de uma imagem, como um prédio construído a partir de uma planta:
- Ambiente isolado e executável
- Estado mutável
- Processos em execução
- Recursos computacionais alocados

## Analogia Matrix 🕶️

```ascii
╔════════════════════════════════════════════════════════════════╗
║ IMAGEM                          CONTAINER                      ║
║ ═══════                         ═════════                      ║
║ - Blueprint                     - Instância em execução        ║
║ - Imutável                      - Mutável                      ║
║ - Template                      - Processo vivo                ║
║ - Compartilhável               - Isolado                      ║
║ - Armazenada em disco          - Executa em memória           ║
╚════════════════════════════════════════════════════════════════╝
```

## Roadmap de Aprendizado 🗺️

1. [Fundamentos de Imagens](image-fundamentals.md)
   - Conceitos básicos
   - Componentes principais
   - Características essenciais

2. [Anatomia de uma Imagem](image-anatomy.md)
   - Sistema de layers
   - Manifests e configurações
   - Formato e especificações

3. [Gerenciamento de Imagens](image-management.md)
   - Comandos básicos
   - Tags e versionamento
   - Registries e distribuição

4. [Operações com Containers](container-operations.md)
   - Ciclo de vida
   - Comandos essenciais
   - Estados e transições

5. [Melhores Práticas](best-practices.md)
   - Guidelines de segurança
   - Otimizações
   - Padrões recomendados

6. [Laboratório Prático](hands-on-lab.md)
   - Exercícios guiados
   - Casos de uso reais
   - Desafios práticos

7. [Guia de Troubleshooting](troubleshooting-guide.md)
   - Problemas comuns
   - Soluções
   - Debugging

## Quick Start ⚡

```bash
# Baixar uma imagem
docker pull nginx:latest

# Criar e executar um container
docker run -d -p 80:80 nginx:latest

# Listar containers em execução
docker ps

# Parar o container
docker stop <container-id>
```

> **Image-chan diz:** "Pense em mim como seu template perfeito - imutável e confiável! uwu"
{style="tip"}

> **Container-kun avisa:** "E eu sou a manifestação viva do seu código - ativo e dinâmico! 🚀"
{style="note"}

## Próximos Passos 🎮

Explore cada subtópico na ordem sugerida para uma jornada de aprendizado estruturada. Comece com os [Fundamentos de Imagens](image-fundamentals.md) para construir uma base sólida.

> **Sensei's Corner:** "A jornada de mil containers começa com uma única imagem!"
{style="warning"}