
![[Pasted image 20261028232644.png]]

### start_time:
The timestamp from which the scheduler will attempt to backfill

O cron agenda execuções em momentos específicos do relógio e do calendário, enquanto o time delta se baseia exclusivamente no intervalo exato de tempo físico decorrido entre elas.

cron é melhor quando se tem que fazer algo em alguma data especifica, como toda segunda, enquanto timedelta é mais interessante quando o tempo da proxima execução depende restritamente da anterior (por exemplo: execução a cada 3 dias)

catchup=true:
Permite executar dags retroativamente desde a ultima execução até o dia atual

![[Pasted image 20261028235755.png]]

### Backfill

permite rodar a dag por um tempo definido de inicio e fim.

com airflow CLI:
airflow dags backfill --start-date START_DATE --end-date END_DATE dag_id
ou pela interface da para fazer.

Veja um cenário onde catchup=true
![[Pasted image 20261029000340.png]]
 
 Decidimos fazer backfill de 24 até 26. 
![[Pasted image 20261029000247.png]]

### Xcoms:**
Compartilha dados entre tasks

![[Pasted image 20261029142313.png]]

os dados são jogados para o metadata datababe
com um identificador como uma key, run_id, task_id e dag_id.

Na maioria das vezes, somente a key e o task_id são suficientes para identificar esse dado.

Dados em xcom tem que ser metadados, isso porque o metadata database não foi arquitetado para armazenar volumes grandes de dados e sim como o nome diz: metadados. Segue limitações de uso, se o metadado for:
**sqlite -> 2gb maximo** 
**postgresql -> 1gb maximo**
**mysql -> 64mb maximo**
os dados do xcom devem ser JSON serializable. 
xcom faz push ("insert" no metadata database) e pull ("select" na base).