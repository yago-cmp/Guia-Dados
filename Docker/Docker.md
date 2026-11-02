### Container
***
Um container é uma unidade padrão de software, que agrupa **código e dependências**, com o objetivo de oferecer velocidade e confiança entre diversos ambientes operacionais. Um container docker é um pacote leve, sem interdependências, que contém tudo que o programa precisa para rodar.

### Container Image
***
A imagem é um arquivo estático que contém justamente todo o conteúdo: código, bibliotecas, dependências, etc. Ela é **imutável** (para cada alteração ela precisa ser gerada novamente). A imagem é construída com base em camadas de sistema sobrepostas, permitindo a reutilização de componentes ou dependências. 

> [!Timeline]
> **Dockerfile** -> Docker **Image** -> Docker **Container**

### Docker Engine
***
Docker Engine é o sistema que roda para criar, gerenciar e interagir com imagens e containers. Ele usa um Daemon (dockerd), um processo de servidor que roda em background e tem como "língua" as REST APIs. Todo tipo de comunicação, como pelo CLI Docker, é enviada via REST para o container em si, que interpreta e performa a ação.

### Containers vs VMs
***
Containers são abstrações no nível de aplicação, enquanto que VMs são abstrações a nível de hardware físico. Diversos containers podem rodar em cima de um único sistema operacional.