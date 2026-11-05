quando rodando pelo docker, iniciar um bash dentro do scheduler para rodar os comandos:
#### `docker exec -it <nome_container> bash`

## airflow -h
lista com todos os comandos
## airflow cheat-sheet
lista com comandos mais comuns
## airflow standalone
cria uma execução de airflow SEM CONTAINERS, em apenas 1 processo para pequenos testes (versão leve) - fechar o terminal acaba com a execução
## airflow users create -e -f -l -u -p -r
email, first name, last nme, user, password, role
## airflow tasks test <dag_id> <task_id> <logical_date>
testa e roda uma task temporaria, mas sem salvar nada no db e sem salvar os Xcoms.]

## airflow dags backfill <dag_id> -s <start_date> -e <end_date>
faz um backfill da dag em questao no intervalo especificado

## airflow dags list-import-errors
lista todos os erros de parsing das dags, bom para debugging quando a dag não aparece na UI

## airflow dags report
mostra o tempo de parsing para cada dag, para identificar o que causa lentidão ao sistema