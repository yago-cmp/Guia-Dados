o Dynamic Task Mapping é uma ferramenta para criar e rodar tasks de forma dinâmica, muitas vezes produzindo cópias de um modelo pré-selecionado de task. O caso de uso principal é quando não se sabe exatamente a quantidade, nomes ou as especificidades das execuções.

### .expand( )
O expand é usado para multiplicar instâncias de dags. Na UI do Airflow, ela viram uma coletânea, que é indicada pelos colchetes [] logo após o seu nome. O expand aceita como parâmetro dicionários, listas e parâmetros XCom.

No exemplo abaixo, uma task simples é multiplicada por 3, ao passar a coleção de valores possíveis para o parâmetro ´File´.
```
@dag(schedule=None, dag_id="exemplo_expand")
def exemplo_expand():
	@task
	def printar_arqs(file: str)
		print(file)
		
	arquivos = printar_arqs.expand(file=["ola", "fui", "expandido"])
```

Adicionando o código abaixo, agora não sabemos exatamente quantas ocorrências da task ocorrerão, pois elas dependem do tamanho da lista gerada pela task anterior.
```
	@task get_files():
		return ["arq_{n}" for n in range(random.randint(1,5))]
		
	# definição da ordem de precedencia implicita
	arquivos = printar_arqs.expand(file=get_files())
```

### .partial( )
É possível usar o .partial() para passar como parâmetro valores que não mudam, como chaves, nomes de projeto, constantes, etc. No exemplo abaixo, o valor parcial Y é passado para todas as taks na expansão `x=[1,2,3]`.
```
added_values = add.partial(y=10).expand(x=[1, 2, 3])
# This results in add function being expanded to
# add(x=1, y=10)
# add(x=2, y=10)
# add(x=3, y=10)
```

É possível definir pelo partial a **quantidade máxima de execuções** paralelas via expand de uma task, cujo **default é 16**.
```
Operator.partial(max_active_tis_per_dag=16).expand()
```