# Principais Comandos do `Dockerfile` no Dia a Dia 

---

## 1. Estrutura Base da Imagem

### `FROM`
Define a imagem base a partir da qual você vai construir a sua. É sempre o primeiro comando real do `Dockerfile`.
* **No seu projeto:** `FROM python:3.12-alpine`
* **Dica para Engenharia de Dados (`alpine` vs `slim`):**
  * `-alpine`: Extremamente leve, mas usa uma biblioteca C diferente (`musl`). Às vezes, bibliotecas pesadas de dados (`pandas`, `numpy`, `pyarrow`, `psycopg2`) demoram muito para instalar no Alpine porque precisam compilar código C do zero (por isso seu `Dockerfile` precisou instalar `gcc` na linha 14!).
  * `-slim` (ex: `FROM python:3.12-slim`): **A mais recomendada para Engenharia de Dados.** É baseada em Debian enxuto, continua sendo leve, mas já aceita os pacotes pré-compilados (*wheels*) do `pandas`, `pyarrow` e drivers de banco sem precisar compilar nada.

### `WORKDIR`
Define a pasta padrão dentro do container onde os próximos comandos (`RUN`, `COPY`, `CMD`) serão executados. Se a pasta não existir, ele cria automaticamente.
* **No seu projeto:** `WORKDIR /code` (ou `/app`, muito comum em projetos de dados).

---

## 2. Instalação de Dependências e Arquivos

### `RUN`
Executa um comando de terminal Linux **durante a construção da imagem** (momento do `build`), criando uma nova camada na imagem.
* **Quando usar:**
  1. Instalar pacotes do sistema operacional (drivers ODBC do SQL Server, cliente `libpq-dev` do Postgres, Java para PySpark, `curl`, `git`).
  2. Instalar bibliotecas Python com `pip install`.
* **No seu projeto:**
  ```dockerfile
  RUN apk add --no-cache gcc musl-dev linux-headers
  RUN pip install -r requirements.txt
  ```
* **Dica de ouro:** Sempre use `pip install --no-cache-dir -r requirements.txt` para não guardar arquivos temporários de download do pip dentro da imagem.

### `COPY`
Copia arquivos e pastas da sua máquina (Windows) para dentro da imagem no momento do build.
  ```dockerfile
  COPY requirements.txt .
  RUN pip install -r requirements.txt
  COPY . .
  ```
  O Docker guarda **cache** de cada linha. Se você copiar só o `requirements.txt` primeiro, o Docker só vai rodar o `pip install` de novo se você adicionar uma biblioteca nova. Se você alterar apenas o `app.py`, o build leva menos de 1 segundo!

---

## 3. Configuração e Execução

### `ENV`
Define variáveis de ambiente permanentes dentro da imagem.
* **No seu projeto:**
  ```dockerfile
  ENV FLASK_APP=app.py
  ENV FLASK_RUN_HOST=0.0.0.0
  ```
* **Duas variáveis `ENV` muito usadas em Python para Dados:**
  * `ENV PYTHONUNBUFFERED=1`: Força o Python a mostrar os `print()` e `logging` em tempo real no `docker compose logs` (sem guardar em buffer). Essencial para acompanhar logs de ETL!
  * `ENV PYTHONDONTWRITEBYTECODE=1`: Impede o Python de criar pastas `__pycache__` e arquivos `.pyc` dentro do container.

### `ARG` (Diferença para `ENV`)
Define variáveis que só existem **durante o `build`** da imagem (não ficam salvas quando o container estiver rodando).
* **Quando usar:** Para passar versões de pacotes ou tokens de build sem deixá-los expostos no container final.

### `EXPOSE`
Documenta qual porta o serviço dentro do container utiliza.
* **No seu projeto:** `EXPOSE 5000`
* **Lembrete:** Não publica a porta no Windows sozinho; serve apenas como documentação técnica da imagem.

### `CMD` vs `ENTRYPOINT` (Por que o Dockerfile precisa de um deles para funcionar?)

#### A Regra de Ouro: Container = 1 Processo (`PID 1`)
Diferente de uma Máquina Virtual (que liga um Windows/Linux inteiro e fica esperando), **um container Docker só existe enquanto o comando principal dele estiver rodando**.
* Se você criar um `Dockerfile` sem `CMD` e sem `ENTRYPOINT` (e a imagem base não tiver um), o container liga e desliga no mesmo milissegundo.
* **Para servidores/bancos (Flask, Redis, Postgres, Airflow Webserver):** O comando no `CMD` fica rodando em loop infinito esperando conexões, mantendo o container vivo (`Up`).
* **Para scripts de Engenharia de Dados (Jobs de ETL):** Se o seu `CMD` for `["python", "extrair_dados.py"]`, o container vai ligar, rodar a extração e, assim que o script terminar a última linha de código, o container vai desligar sozinho (`Exited (0)`).

#### Como `ENTRYPOINT` e `CMD` funcionam juntos?
Você pode usar apenas um deles (o mais comum é usar só `CMD`), ou usar **os dois juntos**. Quando os dois existem no `Dockerfile`, o Docker **soma os dois**: o `ENTRYPOINT` vira o comando fixo e o `CMD` vira o argumento passado para ele.

* **Exemplo com `tini` (gerenciador de processo PID 1):**
  ```dockerfile
  ENTRYPOINT ["/sbin/tini", "--"]
  CMD ["node", "server.js"]
  # Comando real executado ao ligar o container:
  # /sbin/tini -- node server.js
  ```
* **Exemplo clássico em Engenharia de Dados (Container como executável de Pipeline):**
  ```dockerfile
  ENTRYPOINT ["python", "ingestao.py"]
  CMD ["--tabela", "clientes"]
  # Se rodar só 'docker compose up', ele executa: python ingestao.py --tabela clientes
  # Se rodar 'docker compose run web --tabela vendas', ele substitui só o CMD: python ingestao.py --tabela vendas
  ```

---

## 4. Comandos Avançados que você verá em outros projetos

### `FROM imagem AS apelido` (Multi-stage Build)
Exemplo: `FROM node:12-alpine AS dist`
* **Para que serve:** Dá um nome (`dist`) para aquela etapa de construção. Permite ter múltiplos `FROM` no mesmo `Dockerfile`: uma primeira etapa pesada com compiladores (`gcc`, `maven`, `webpack`) que gera o pacote final, e uma segunda etapa limpa que copia apenas os arquivos prontos (`COPY --from=dist ...`), deixando a imagem final muito menor.

### `HEALTHCHECK`
Exemplo: `HEALTHCHECK CMD node /app/healthcheck.js || exit 1`
* **Para que serve:** Ensina o Docker a testar periodicamente se a aplicação dentro do container ainda está saudável (e não travada).
* Se o comando retornar código `0`, o container fica com status `(healthy)`. Se retornar `1` (`exit 1`), fica `(unhealthy)`.

---

## Resumo Rápido das Instruções do `Dockerfile`

| Instrução | Momento em que age | Para que serve |
| :--- | :--- | :--- |
| `FROM ... [AS nome]` | Build | Escolhe a imagem base (e opcionalmente dá um apelido para a etapa) |
| `WORKDIR` | Build / Run | Cria e entra na pasta de trabalho (ex: `/code` ou `/app`) |
| `COPY` | Build | Copia arquivos do seu PC para dentro da imagem |
| `RUN` | Build | Roda comandos para instalar pacotes Linux e dependências (`pip`, `npm`) |
| `ENV` | Build e Run | Cria variáveis de ambiente fixas no container |
| `EXPOSE` | Documentação | Indica em qual porta interna a aplicação escuta |
| `HEALTHCHECK` | Run (periódico) | Testa de tempos em tempos se o serviço dentro do container está respondendo |
| `ENTRYPOINT` | Run (quando liga) | Define o executável principal fixo do container |
| `CMD` | Run (quando liga) | Define o comando padrão (ou os argumentos padrões para o `ENTRYPOINT`) |


