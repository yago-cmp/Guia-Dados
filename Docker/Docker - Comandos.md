### Lista de comandos do **CLI Docker**!

***
# Básicos 

## Pull
Baixa uma Imagem, por padrão do Dockerhub.
- `docker pull <image>`
     Ex: `docker pull docker/welcome-to-docker`
## Build
Transforma um Dockerfile em uma Imagem.
- `docker build -t <nome> <caminho>`
	 Ex: `docker build -t imagem-ola .`
## Create
Cria um container a partir de uma imagem.
`docker create --name <nome-container> <imagem>`
## Run
Roda um container a partir de uma Imagem. **Combina o docker build com o docker create**.
- `docker run (--rm) (-e ) (-d) (-it) (-p <porta_host:porta_container>) --name <nome_container> <nome_imagem> <comando>`
	 `--rm`: remove, remove o container após de rodar (importante!)
	 `-d`: detach, roda container no **background**
	 `-p <porta_host:porta_container>`: publish, **mapeia uma porta** do host a uma porta interna do container
	 `-e`: define variaveis de ambiente
	- `-v`: aloca um volume a um 
	- `-it`: deixa o terminal aberto, iterativo
	- `comando`: roda algum comando, sobrescreve o CMD do Dockerfile
## List
Lista imagens e containers do computador.
- `docker image ls`
- `docker container ls`
## Stop
Para de executar um container.
- `docker container stop <nome_container>
## Remove
Remove imagens e containers do computador.
- `docker rm --force <nome_container>`: stop + remove container
- `docker container rm <nome_container>`
- `docker image rm <nome_imagem>`
- 
***
# Compose
O Compose tem como base o **docker-compose.yaml** que define tudo com relação a uma execução específica. Ele serve para orquestração, ou seja, é possível definir diversos containers para rodarem de uma determinada forma dentro do compose.yaml.
## Compose Up
Faz build das imagens (se necessário) e dá run em todos os containers especificados.
- `docker compose up (-d) (--build)
	 `-d`: detached, roda os containers no background
	 `--build`: força a reconstrução de todas as imagens antes de rodar
## Compose Down
Interrompe e remove todos os containers especificados.
- `docker compose down (-v)
	 `-v`: volumes, remove também os volumes de dados associados
## Compose Stop
pausa todos os containers especificados
- `docker compose stop`
## Compose Start
retoma a execução de todos os containers especificados
- `docker compose start
## Compose Logs
Lista as mensagens e outputs gerados pelos containers especificados.
- `docker compose logs (-f) (<nome_servico>)`
	 `-f`: follow, mostra em tempo real as mensagens
	 `<nome_servico>`: mostra

***
# Volume Mounts
Um Volume é uma unidade de dados persistentes, que fica na memória mesmo após o fim de um container.
## Volume Create
Cria um volume independentemente de containers.
`docker volume create <nome_volume>`
## Volume List
Lista todos os volumes.
`docker volume ls`
## Criar/Acessar Volume no Run
Cria ou acessa um volume pré-existente através do docker run.
`docker run -d -p 80:80 -v <nome_volume>:/<nome_pasta> <imagem>` 
      `-v <volume>:/<pasta>`: cria o volume na pasta X do sistema.
OU
`docker run -d --mount type=volume, source=<nome_mount>, target=<caminho> <imagem>`
	 **type pode ser volume, bind ou tmpfs**
## List, Remove, Prune
`docker volume ls`: lista todos os volumes
`docker volume rm <volume>`: remove o volume (se não estiver em uso)
`docker volume prune`: remove todos os volumes sem uso

***
# Bind Mounts
Mapeiam uma parte específica (uma pasta, path) do computador host para ter acesso de dentro do container. Útil se um arquivo está sendo editado no host e precisa ser acessado constantemente dentro do container. Permanece salvo no disco rígido.
`docker run -d --mount type=bind, source=<path_absoluto>, target=<caminho> <imagem>`

***
# Temporary FileSystem Mounts
Os mounts tmpfs ficam na ram do host, e perdura apenas até ele reiniciar. Não é possível compartilhar esse tipo de mounts entre múltiplos containers.

***
# Outros 

## Reference
Define a identidade, a unicidade de uma imagem Docker.
`<HOST<:PORT>/>NAMESPACE/REPOSITORY <:TAG>`
## Tag
Cria uma referência de uma imagem local com um alvo.
`docker image tag <ref_local:tag> <ref_alvo:tag>`
	`tag` é a identificação da versão local e alvo
	
A ideia é de que, definindo **um remetente e um destinatário**, ao fazer `docker push <ref_alvo:tag>`, essa referência é usada para direcionar o docker a onde salvar a imagem.

Exemplo com o **Artifact Registry no GCP**, ao invés do DockerHub:
```
docker tag <ref_local> <location>-docker.pkg.dev/<project_id>/<repository>/<imagem:tag>
```
É preciso estar autenticado usando `gcloud auth configure-docker.
## Logs
Lista todas as mensagens e outputs gerados pelo processo principal do container, o PID 1, no stdout ou stderr.
- `docker logs (-f) (-t) (--tail <qtd>) <nome_container>`
	 `-f`: follow, mostra em tempo real as mensagens
	 `-t`: time, mostra o tempo ao lado das mensagens
      `--tail <qtd>`: limit, mostra apenas as últimas X mensagens
## Execute
Executa comandos dentro de um container.
- `docker exec (-it) <nome_container> <comando>` 
	 `-it`: mantém aberto, com a linha de comando


Exemplo de **Bind Mount**:
```
docker run -d --mount type=bind,source="C:\Users\yago.campello_ipnet\Downloads\para testar",target=/app/data --name container_ola --rm ola_arquivo

docker exec -ti container_ola bash 

docker image rm imagem_ola  
```
