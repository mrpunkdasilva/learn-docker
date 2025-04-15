# Anatomia de uma Imagem Docker 🔬

Uma imagem Docker é composta por várias camadas sobrepostas que formam um sistema de arquivos unificado. Vamos dissecar cada componente.

## Visão Geral da Estrutura

```ascii
╔══════════════════════════════════════════════════════════════════╗
║ IMAGE ANATOMY MATRIX                                             ║
║                                                                  ║
║    ┌─────────────┐                                              ║
║    │   Config    │                                              ║
║    └─────────────┘                                              ║
║           ↓                                                      ║
║    ┌─────────────┐     ┌─────────────┐                         ║
║    │  Manifest   │────▶│  Metadata   │                         ║
║    └─────────────┘     └─────────────┘                         ║
║           ↓                                                      ║
║    ┌─────────────┐                                              ║
║    │   Layers    │                                              ║
║    └─────────────┘                                              ║
║                                                                 ║
╚══════════════════════════════════════════════════════════════════╝
```

## 1. Arquivo de Configuração

O arquivo de configuração é um documento JSON que define o comportamento da imagem.

### Componentes Principais

```json
{
  "config": {
    "Hostname": "",
    "Domainname": "",
    "User": "",
    "ExposedPorts": {
      "80/tcp": {}
    },
    "Env": [
      "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
    ],
    "Cmd": [
      "nginx",
      "-g",
      "daemon off;"
    ],
    "WorkingDir": "/",
    "Volumes": null
  }
}
```

### Elementos Essenciais
- `ExposedPorts`: Portas que o container expõe
- `Env`: Variáveis de ambiente padrão
- `Cmd`: Comando padrão executado no container
- `WorkingDir`: Diretório de trabalho inicial
- `User`: Usuário padrão do container

## 2. Manifesto da Imagem

O manifesto é um documento JSON que descreve a composição da imagem.

### Estrutura do Manifesto

```json
{
  "schemaVersion": 2,
  "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
  "config": {
    "mediaType": "application/vnd.docker.container.image.v1+json",
    "size": 1234,
    "digest": "sha256:a1b2c3..."
  },
  "layers": [
    {
      "mediaType": "application/vnd.docker.image.rootfs.diff.tar.gzip",
      "size": 5678,
      "digest": "sha256:d4e5f6..."
    }
  ]
}
```

### Elementos do Manifesto
- `schemaVersion`: Versão do formato do manifesto
- `config`: Referência ao arquivo de configuração
- `layers`: Lista de camadas que compõem a imagem
- `digest`: Hash SHA256 único da camada

## 3. Sistema de Camadas

As camadas são organizadas em uma estrutura de pilha, onde cada camada representa uma modificação no sistema de arquivos.

```ascii
╔════════════════════════════════════════════════════════════╗
║ LAYER STACK                                                ║
║                                                           ║
║ ┌────────────────────┐                                    ║
║ │   App Layer        │ <- Código da aplicação             ║
║ ├────────────────────┤                                    ║
║ │   Dependencies     │ <- Bibliotecas e deps              ║
║ ├────────────────────┤                                    ║
║ │   Runtime          │ <- Ambiente de execução            ║
║ ├────────────────────┤                                    ║
║ │   Base OS          │ <- Sistema operacional base        ║
║ └────────────────────┘                                    ║
║                                                           ║
╚════════════════════════════════════════════════════════════╝
```

### Características das Camadas

#### Camadas Read-Only
- Imutáveis após criação
- Compartilhadas entre containers
- Armazenadas em cache localmente

#### Camada Read-Write
- Criada ao iniciar um container
- Específica para cada container
- Contém todas as alterações em runtime

## 4. Sistema de Arquivos Union

O Docker utiliza um sistema de arquivos em camadas que combina todas as camadas em uma única visão.

```ascii
╔════════════════════════════════════════════════════════════╗
║ UNION FILESYSTEM                                           ║
║                                                           ║
║ /container                                                ║
║   ├── /bin   ┐                                           ║
║   ├── /etc   │                                           ║
║   ├── /lib   ├─ União de todas as camadas                ║
║   ├── /usr   │                                           ║
║   └── /var   ┘                                           ║
║                                                           ║
╚════════════════════════════════════════════════════════════╝
```

### Funcionamento
1. Cada camada representa um conjunto de diferenças
2. As camadas são montadas sobrepostas
3. Arquivos mais recentes têm precedência
4. Deleções são marcadas com whiteouts

## 5. Comandos de Inspeção

### Análise de Imagens
```bash
# Metadata da imagem
docker inspect nginx:latest

# Histórico de camadas
docker history nginx:latest

# Digest da imagem
docker images --digests nginx

# Exportar imagem
docker save nginx:latest > nginx.tar
```

## Waifu Tips 💡

> **Layer-chan diz:** "Cada camada é como um capítulo da história do seu container. Mantenha-a interessante, mas concisa! 📚"
{style="tip"}

> **Storage-chan avisa:** "Cuidado com o copy-on-write! Muitas escritas podem impactar a performance! 🚀"
{style="warning"}

## Próximos Passos

1. [Gerenciamento de Imagens](image-management.md)
2. [Operações com Containers](container-operations.md)
3. [Melhores Práticas](best-practices.md)