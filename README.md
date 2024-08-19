![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)

# Formatando o README.md
-  [Formatação MarkDown](https://docs.github.com/pt/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)
- https://shields.io/
- https://simpleicons.org/


# Explorando o Docker

- O que são containers?
    - Containers são uma forma de empacotar aplicações com todas as suas dependências, de forma que possam ser executadas de forma isolada em qualquer ambiente.
- O que é Docker?
    - Docker é uma plataforma de código aberto que facilita a criação, execução e gerenciamento de containers.
- Que linguagem de programação foi utilizada para desenvolver o Docker?
    - Go -> https://golang.org/
- O que é Docker Hub?
    - Docker Hub é um serviço de registro de imagens Docker, onde é possível encontrar imagens prontas para serem utilizadas.
- O que é Docker Compose?
    - Docker Compose é uma ferramenta que permite a definição e execução de aplicações multi-container Docker.


# Conteiners

- Namespace
    - Namespace é um recurso do kernel do Linux que permite isolar processos, redes, sistemas de arquivos, entre outros recursos do sistema operacional.
        - PID: Isola processos
            - `ps aux`
        - NET: Isola interfaces de rede
            - `ip a`
        - IPC: Isola mecanismos de comunicação entre processos
        - MNT: Isola sistemas de arquivos
        - UTS: Isola identificadores de host e domínio
        - USER: Isola usuários
- Cgroups (Criado pelo Google)
    - Cgroup: Isola recursos de hardware dos containers
        - CPU
            - `cat /proc/cpuinfo`
        - Memória
            - `cat /proc/meminfo`
        - Disco
        - Rede
- Resumindo, containers são processos isolados que compartilham o kernel do sistema operacional.

- 3 Pilares de Containers
    - Namespace
        - Isola processos, redes, sistemas de arquivos, entre outros recursos do sistema operacional.
            - Ex: PID, NET, IPC, MNT, UTS, USER.
    - Cgroups
        - Isola recursos de hardware dos containers.
            - Ex: CPU, Memória, Disco, Rede.
    - OverlayFS
        - OverlayFS é um sistema de arquivos que permite a criação de camadas de arquivos, facilitando a criação de imagens Docker.
            - Ex: Camadas(Layer) de imagens Docker.

- Diferença entre Containers e VMs
    - Containers
        - Compartilham o kernel do sistema operacional.
        - São mais leves e rápidos.
        - São mais fáceis de escalar.
    - VMs
        - Possuem um sistema operacional completo.
        - São mais pesadas e lentas.
        - São mais difíceis de escalar.

- Como conteiner trabalha com `layers`, ele é mais rápido e mais leve que uma VM.
    - Ex: temos a versão MyApp:v1 e a versão MyApp:v2, o container irá criar uma nova camada para a versão MyApp:v2, e irá compartilhar as camadas comuns entre as versões.
    - Portanto, chamamos isso de reutilização de camadas.

- Cada processo dentro de um container é um processo do sistema operacional `host` e dentro do container eu posso ter `N`processos rodando de maneira isolada.    
    - Ex: `ps aux`

- Estado representacional de execução de um container
    - Sistema Operacional Host 
        - Docker Engine
            - Container -> Processo do sistema operacional host, este não é um processo do container e sim do sistema operacional host
                - Processo 1 -> Aqui é um processo do container ou sub-processo do processo do container.
                - Processo 2 -> idem
                - Processo 3 -> idem
                - Processo N -> idem
            
- O conteiner criado a partir de uma imagem imutável, ou seja, não pode ser alterado.
    - Existe uma camada de READ ONLY e uma camada de READ/WRITE, que é temporária e é descartada quando o container é removido.

> [!NOTE]
> - O conteiner é uma instância de uma imagem, ou seja, é uma cópia da imagem.

> [!WARNING]
> - Se o processo principal do container for finalizado, o container é finalizado, perdendo o estado ou dados que não foram persistidos em um volume de container.

# Imagens

- O que são imagens?
    - Imagens são templates somente leitura que contém instruções para criar um container.
    - Portanto, posso dizer que imagem é enfemerável, ou seja, não pode ser alterada.
    - Para alterar uma imagem, é necessário criar uma nova imagem.
    - Imagem é composta por camadas(Layers).
    - Imagem é composta por um arquivo chamado Dockerfile.
    - O que é Dockerfile?
        - Dockerfile é um arquivo declarativo que contém instruções para criar uma imagem Docker.
        - Exemplo de Dockerfile(Imagem customizada a partir da imagem node:14):
            ```Dockerfile
            FROM node:14
            WORKDIR /app
            COPY . .
            RUN npm install
            CMD ["node", "index.js"]
            EXPOSE 3000
            ```
    - Imagem é um conjunto de camadas(Layers) que são empilhadas uma sobre a outra.
        - Ex: `docker image inspect node:14`
        - Ex: Digamos que a imagem node:14 possui 10 camadas, e a imagem node:16 possui 10 camadas, e 5 camadas são comuns entre as duas imagens, então a imagem node:16 irá criar 5 camadas novas e irá compartilhar as 5 camadas comuns com a imagem node:14.
    - Cada camada é somente leitura e é imutável.

    - As imagens são armazenadas em um registro de imagens(Registry) local ou remoto.
        - Ex: Docker Hub, Amazon ECR, Google Container Registry, Azure Container Registry.
    - Baixar imagem do Docker Hub
        - `docker pull node:14`
    - Listar imagens
        - `docker image ls`
    - Remover imagem
        - `docker image rm node:14`

# Resumo

- Dockerfile 
    - Arquivo declarativo que contém instruções para criar uma imagem Docker.
    ```Dockerfile
    FROM node:14
    WORKDIR /app
    COPY . .
    RUN npm install
    CMD ["node", "index.js"]
    EXPOSE 3000
    ```
- Gerar imagem a partir do Dockerfile (Imutável com a camada de READ ONLY e camada de READ/WRITE)
    - `docker build -t myapp:1.0 .`
- Ou gerar imagem  v2.0 a partir do conteiner em execução (Imutável com a camada de READ ONLY e camada de READ/WRITE)
    - `docker commit myapp myapp:2.0`



# Referências
- https://www.docker.com
- https://docs.docker.com
- https://hub.docker.com