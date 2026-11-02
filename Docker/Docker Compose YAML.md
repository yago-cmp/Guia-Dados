# Principais Comandos (Chaves) do `compose.yml` no Dia a Dia (Engenharia de Dados)

Enquanto o arquivo [comandos-compose.md](file:///c:/Users/mateus.maciel_ipnet/Desktop/Estudos/docker/projeto2.1-docker-compose/comandos-compose.md) mostra os comandos que você digita no terminal, este guia lista **os comandos/instruções que você escreve dentro do arquivo `compose.yml`**, explicando para que servem e quando usá-los no dia a dia de Engenharia de Dados.

---

## 1. Definição e Construção do Serviço

### `services:`
É o bloco raiz onde você lista todos os containers do seu projeto.
* **Para que serve:** Agrupar seus containers (ex: `web`, `redis`, `postgres`, `airflow-scheduler`).
* **Detalhe importante:** O nome de cada serviço abaixo de `services:` vira automaticamente o **hostname (DNS)** dele na rede interna.

### `image:`
Define uma imagem pronta que será baixada do Docker Hub (ou outro registro, como AWS ECR / Google Artifact Registry).
* **Quando usar:** Sempre que você quiser subir um banco de dados ou ferramenta pronta sem precisar modificar o código-fonte dela (ex: `postgres:16`, `redis:alpine`, `apache/spark`, `bitnami/kafka`).
* **No seu projeto:**
  ```yaml
  redis:
    image: redis:alpine
  ```

### `build:`
Diz ao Docker Compose para construir uma imagem a partir de um `Dockerfile` local em vez de baixar pronta.
* **Quando usar:** Quando o container roda **o seu próprio código** (scripts Python de ETL, projetos dbt, APIs) ou quando você precisa instalar bibliotecas extras em uma imagem base.
* **Variações no dia a dia:**
  ```yaml
  # Forma simples (procura o Dockerfile na pasta atual):
  web:
    build: .

  # Forma avançada (quando o Dockerfile tem outro nome ou está em outra pasta):
  web:
    build:
      context: .
      dockerfile: Dockerfile.dev
  ```

### `container_name:`
Define um nome fixo para o container (em vez do nome automático `projeto21-docker-compose-web-1`).
* **Quando usar:** Quando você quer um nome curto e padronizado para rodar comandos rápidos no terminal (ex: `docker exec -it redis_local redis-cli`).

---

## 2. Comunicação, Portas e Variáveis de Ambiente

### `ports:`
Mapeia uma porta do seu computador (Host) para uma porta de dentro do container (`"PORTA_WINDOWS:PORTA_CONTAINER"`).
* **Quando usar:** Apenas quando **você (ou um programa instalado no seu Windows, como DBeaver, Power BI ou o navegador)** precisa acessar o serviço que está dentro do container.
* **No seu projeto:**
  ```yaml
  ports:
    - "${APP_PORT}:5000"
  ```

### `environment:` e `env_file:`
Injetam variáveis de ambiente dentro do container.
* **Quando usar:** Para passar credenciais de banco (`POSTGRES_PASSWORD`), configurações de conexão (`REDIS_HOST`, `REDIS_PORT`), caminhos de buckets (`S3_BUCKET`), etc.
* **Diferença prática:**
  * Use `environment:` para passar poucas variáveis ou valores fixos.
  * Use `env_file:` quando quiser carregar **todas** as variáveis do seu arquivo `.env` de uma só vez sem precisar digitar uma por uma no `compose.yml`:
  ```yaml
  web:
    env_file:
      - .env
  ```

### `networks:`
Cria redes virtuais isoladas ou conecta containers a uma rede já existente.
* **Quando usar em Dados:** Muito útil quando você tem **dois projetos separados** em pastas diferentes (ex: um `compose.yml` só para o Airflow e outro `compose.yml` para um banco Postgres local) e precisa que um converse com o outro.

### `extra_hosts:`
Permite que o container acesse serviços que estão rodando **fora do Docker, direto no seu Windows**.
* **Quando usar:** Seu script Python está rodando dentro do container, mas precisa ler um banco SQL Server ou Postgres que está instalado direto no seu Windows (`localhost` do Windows):
  ```yaml
  web:
    extra_hosts:
      - "host.docker.internal:host-gateway"
  # No Python, basta conectar no host "host.docker.internal"
  ```

---

## 3. Dados, Volumes e Persistência (Crucial para Dados!)

### `volumes:`
Conecta pastas ou discos virtuais ao container. Existem **2 usos principais** no dia a dia:

1. **Persistir dados de bancos (Named Volume):** Evita que você perca suas tabelas e dados quando rodar `docker compose down`.
2. **Espelhar código ou arquivos de dados locais (Bind Mount):** Conecta uma pasta do Windows direto dentro do container. Se você editar o `app.py` ou jogar um arquivo `.parquet` / `.csv` na pasta do Windows, o container enxerga na mesma hora sem precisar de `build`!

* **Exemplo prático no seu projeto:**
  ```yaml
  services:
    web:
      build: .
      volumes:
        - .:/code # Bind Mount: espelha a pasta do projeto para dentro de /code
        - ./dados:/data # Espelha uma pasta local de CSVs/Parquets para o container ler

    redis:
      image: redis:alpine
      volumes:
        - redis_storage:/data # Named Volume: salva os dados do Redis permanentemente

  volumes:
    redis_storage: # Todo Named Volume precisa ser declarado aqui no final
  ```

---

## 4. Execução, Ordem de Subida e Recursos

### `command:` (e `entrypoint:`)
Sobrescreve o comando padrão (`CMD`) que foi definido no `Dockerfile`.
* **Quando usar em Dados:**
  * Reaproveitar a mesma imagem Python para rodar scripts diferentes em serviços diferentes (ex: um serviço roda `python ingestao.py` e outro roda `python transformacao.py`).
  * Passar flags de inicialização para bancos (ex: fazer o Redis salvar dados no disco com `command: redis-server --appendonly yes`).

### `depends_on:` + `healthcheck:`
Controlam a ordem em que os containers sobem e garantem que um banco de dados está **pronto para receber conexões** antes de o script Python tentar conectar.
* **Quando usar:** Sempre que um serviço (`web` / script de ETL / Airflow) depender de um banco de dados (`redis`, `postgres`, `mysql`). Sem isso, o Python sobe rápido demais e falha com `Connection Refused` porque o banco ainda estava carregando.
* **Exemplo prático:**
  ```yaml
  services:
    web:
      build: .
      depends_on:
        redis:
          condition: service_healthy # Só inicia o 'web' após o 'redis' responder ao ping

    redis:
      image: redis:alpine
      healthcheck:
        test: ["CMD", "redis-cli", "ping"]
        interval: 5s
        timeout: 3s
        retries: 5
  ```

### `restart:`
Define a política de reinicialização automática caso o container dê erro e pare.
* **Principais opções:**
  * `no` (padrão): Não reinicia. Ideal para jobs pontuais de ETL que devem rodar e terminar.
  * `unless-stopped` ou `always`: Se cair sozinho, o Docker levanta de novo. Ideal para bancos (`redis`, `postgres`) e serviços contínuos (`web`, `airflow`).

### `deploy:` (Limite de Memória e CPU)
Limita quanto de RAM e processador um container pode consumir da sua máquina.
* **Quando usar em Dados:** Essencial quando você sobe ferramentas pesadas localmente (Spark, Kafka, Trino, banco com milhões de linhas) e não quer que o Docker consuma 100% da RAM do seu computador e trave o Windows:
  ```yaml
  redis:
    image: redis:alpine
    deploy:
      resources:
        limits:
          cpus: "1.0"
          memory: 512M
  ```

---

## Resumo de Consulta Rápida

| Comando no `compose.yml`     | Para que serve                      | Quando usar no dia a dia                                               |
| :--------------------------- | :---------------------------------- | :--------------------------------------------------------------------- |
| `image`                      | Baixa imagem pronta                 | Bancos de dados e ferramentas prontas (`redis`, `postgres`)            |
| `build`                      | Compila um `Dockerfile`             | Seus projetos em Python, dbt, imagens customizadas                     |
| `ports`                      | Abre porta no seu PC                | Acessar UI no navegador ou conectar DBeaver no banco                   |
| `environment` / `env_file`   | Define variáveis de ambiente        | Passar senhas, hosts (`REDIS_HOST`), chaves de nuvem                   |
| `volumes`                    | Salva dados ou espelha pastas       | Não perder dados do banco / ler arquivos `.csv` ou `.py` em tempo real |
| `command`                    | Muda o comando que o container roda | Rodar um script `.py` específico ou passar flags para o banco          |
| `depends_on` + `healthcheck` | Espera o banco ficar pronto         | Evitar erro de conexão quando o Python sobe antes do banco             |
| `restart`                    | Reinicia container se cair          | Manter bancos e servidores ativos (`unless-stopped`)                   |
| `deploy.resources.limits`    | Trava uso máximo de RAM/CPU         | Evitar que Spark/Bancos travem sua máquina local                       |
