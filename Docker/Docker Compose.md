# Guia Prático de Docker Compose para Engenharia de Dados

Este guia reúne os comandos e conceitos de **Docker Compose** que realmente importam no dia a dia de um **Engenheiro de Dados** (subir bancos locais, testar pipelines, inspecionar processos no Airflow/Spark/Postgres/Redis e gerenciar volumes de dados sem perder estado acidentalmente).

---

## 1. Ciclo de Vida dos Containers (Subir e Derrubar)

### `docker compose up`
Cria a rede, os volumes e inicia todos os containers definidos no `compose.yml`.
* **Quando usar:** Para iniciar seu ambiente de desenvolvimento (ex: subir um banco Postgres + Redis + sua aplicação Python).
* **Variações essenciais:**
  * `docker compose up -d`: **(Mais usado)** Roda em *background* (modo *detached*), liberando o seu terminal.
  * `docker compose up --build`: Força a reconstrução da imagem (`Dockerfile`) antes de subir. **Crucial** quando você altera o `requirements.txt`, o `Dockerfile` ou arquivos copiados via `COPY`.
  * `docker compose up redis -d`: Sobe **apenas** o serviço `redis` (útil quando você quer rodar seu script Python localmente na sua máquina, mas precisa apenas do banco rodando no Docker).

### `docker compose down`
Para os containers e **remove** os containers e a rede virtual criada.
* **Quando usar:** Quando terminar os estudos/trabalho do dia e quiser liberar memória RAM e CPU.
* **Variações essenciais (Atenção em Engenharia de Dados!):**
  * `docker compose down`: Derruba os containers, mas **preserva os volumes** (seus dados no banco continuam salvos).
  * `docker compose down -v`: Derruba tudo e **APAGA OS VOLUMES** de dados. Use quando quiser "resetar" um banco de dados corrompido ou começar um teste de ingestão do zero absoluto.

### `docker compose stop` / `docker compose start`
Apenas pausa/desliga os containers existentes sem destruí-los, e depois os religa.
* **Quando usar:** Quando você configurou algo manual dentro do container temporariamente e quer apenas desligar para economizar memória RAM sem recriar o container do zero.

### `docker compose restart [serviço]`
Reinicia todos os serviços ou um serviço específico.
* **Quando usar:** Quando você mudou uma configuração que exige reinicialização do processo (ex: `docker compose restart web`). *Nota: Mudanças no `compose.yml` ou `.env` exigem `docker compose up -d`, o `restart` não recarrega variáveis do `.env`.*

---

## 2. Inspeção, Debug e Acesso Interno (Troubleshoot de Pipelines)

Na Engenharia de Dados, você raramente fica olhando para uma tela web; você precisa ver logs de jobs e entrar no banco para checar tabelas/chaves.

### `docker compose ps`
Lista os containers do projeto atual, mostrando quais estão rodando (`Up`), quais morreram (`Exited`) e quais portas estão mapeadas.
* **Quando usar:** Primeira coisa a rodar quando "a conexão recusou". Verifica se o banco realmente está de pé.

### `docker compose logs`
Mostra a saída (stdout/stderr) dos containers.
* **Quando usar:** Quando um container cai logo após subir ou um job dá erro.
* **Variações essenciais:**
  * `docker compose logs -f`: Segue os logs em tempo real (*follow*).
  * `docker compose logs -f web`: Mostra em tempo real apenas os logs do container `web` (ignora o barulho dos outros serviços).
  * `docker compose logs --tail 50 redis`: Mostra apenas as últimas 50 linhas de log do `redis`.

### `docker compose exec <serviço> <comando>`
Executa um comando **dentro** de um container que já está rodando.
* **Quando usar:** Para abrir um terminal dentro do container, rodar o cliente de um banco de dados (`psql`, `redis-cli`, `spark-shell`) ou testar conectividade.
* **Exemplos neste projeto:**
  * Entrar no shell Linux do container Python:
    ```bash
    docker compose exec web sh
    ```
  * Entrar direto no cliente do Redis para ver quantas visitas estão salvas no banco:
    ```bash
    docker compose exec redis redis-cli
    # Dentro do redis-cli, digite: GET hits
    ```

### `docker compose run --rm <serviço> <comando>`
Cria um container **novo e temporário** para rodar um comando pontual e já o apaga ao finalizar (`--rm`).
* **Quando usar:** Muito comum para tarefas *one-off* (pontuais) em dados: rodar `dbt run`, executar uma migração de schema, rodar um script de ingestão (`python ingest.py`) ou validar testes (`pytest`).
* **Exemplo neste projeto:**
  ```bash
  docker compose run --rm web python -c "import redis; print(redis.__version__)"
  ```

---

## 3. Validação e Imagens

### `docker compose config`
Lê o seu `compose.yml`, substitui todas as variáveis do arquivo `.env` e imprime o YAML final resolvido no terminal.
* **Quando usar:** Quando você está na dúvida se o Docker Compose está lendo corretamente o seu `.env` (ex: `${APP_PORT}`, `${REDIS_HOST}`). Se houver erro de sintaxe no YAML, ele avisa aqui.

### `docker compose build`
Apenas constrói (ou reconstrói) as imagens dos serviços que possuem a cláusula `build:` (como o serviço `web`), sem iniciar os containers.
* **Quando usar:** Quando você está testando se o seu `Dockerfile` está instalando todas as dependências (como drivers ODBC, Java para PySpark ou pacotes Python) sem erros.
* **Variação útil:** `docker compose build --no-cache` (força o download e instalação de tudo do zero, ignorando o cache do Docker).

---

## Resumo Rápido: Qual comando usar em cada situação?

| Situação | Comando |
| :--- | :--- |
| "Quero subir meu ambiente em segundo plano" | `docker compose up -d` |
| "Mudei o `Dockerfile` ou `requirements.txt`" | `docker compose up -d --build` |
| "Mudei uma variável no `.env`" | `docker compose up -d` |
| "Quero ver se minhas variáveis do `.env` funcionaram" | `docker compose config` |
| "O pipeline deu erro, quero ver o que aconteceu" | `docker compose logs -f web` |
| "Quero consultar os dados direto no Redis" | `docker compose exec redis redis-cli` |
| "Quero desligar tudo mas manter os dados salvos" | `docker compose down` |
| "Quero zerar o banco de dados e apagar tudo" | `docker compose down -v` |

