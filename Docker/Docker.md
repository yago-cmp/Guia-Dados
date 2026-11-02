Docker é uma plataforma para containerização de instâncias de código. Ela possui a [[Docker CLI]] e um programa local, o [[Docker Desktop]].

### Container
Um container é uma unidade padrão de software, que agrupa **código e dependências**, com o objetivo de oferecer velocidade e confiança entre diversos ambientes operacionais. Um container docker é um pacote leve, sem interdependências, que contém tudo que o programa precisa para rodar. É uma **instância de uma [[#Container Image]]**, que de fato existe.
***
### Container Image
A imagem é um arquivo **estático** que contém justamente todo o conteúdo: código, bibliotecas, dependências, etc. Ela é **imutável** (para cada alteração ela precisa ser gerada novamente). A imagem é construída, gerada pelo [[#Docker Engine]] através de um arquivo **[[Dockerfile]]**.

O esquema de instanciação padrão de um Container é feito, então, da seguinte forma:
> [!Instanciação]
> Configurar **Dockerfile** -> Gerar Docker **Image** -> Instanciar Docker **Container**

É possível e recomendado utilizar imagens pré-prontas, e muitas estão disponíveis por padrão no DockerHub. Esse processo de importação e alteração de imagens é feito justamente no [[Dockerfile]].
***
### Compose
O [[Docker Compose]] é uma ferramenta para compor uma aplicação com diversos containers. Ele utiliza o CLI e um arquivo YAML ([[Docker Compose YAML]]) para definir e rodar "serviços" (containers) de um conjunto maior, seja uma aplicação ou processo complexo.


***
### Docker Engine
Docker Engine é o sistema que roda para criar, gerenciar e interagir com imagens e containers. Ele usa um Daemon (dockerd), um processo de servidor que roda em background e tem como "língua" as REST APIs. Todo tipo de comunicação, como pelo CLI Docker, é enviada via REST para o container em si, que interpreta e performa a ação.

***
### Containers vs VMs
Containers são abstrações no nível de aplicação, enquanto que VMs são abstrações a nível de hardware físico. Diversos containers podem rodar em cima de um único sistema operacional.