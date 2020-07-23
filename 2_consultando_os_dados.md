## Limpar console no MongoDB

	> tecla control + l

## Consultando e filtrando dados

Lista todos os alunos

	> db.alunos.find()
	
Exibe todos alunos de forma legível

	> db.alunos.find().pretty()
	
Buscando pela chave nome com valor Maria

	> db.alunos.find({
	  nome : "Maria"
	})
		
Inserindo aluno com nome Maria 	

	> db.alunos.insert({
	  nome : "Maria",	
	  curso : {
		  "nome" : "Moda"
	  }
	})
	
Buscando pela chave nome com valor Maria de forma legível (há dois alunos com este nome)

	Similar no sql: SELECT * FROM alunos WHERE nome = "Maria"
	> db.alunos.find({
	  nome : "Maria"
	}).pretty()
	
Removendo um aluno pelo id

	> db.alunos.remove({
	 _id : ObjectId("5f18544a8c7a731623ad4d4c")
	})
	
Filtrando por um campo

	> db.alunos.find({
		"habilidades.nome": "Inglês"
	}).pretty()
	
Filtrando por mais de um campo

	Filtrando com mais de um campo
	> db.alunos.find({
		"nome" : "Pedro",
		"habilidades.nome": "Inglês"
	}).pretty()
	
Filtrando com OR: alunos cujo curso seja "Moda" ou "Engenharia Civil"

	> db.alunos.find({
		$or : [
			{"curso.nome" : "Moda"},
			{"curso.nome" : "Engenharia Civil"}
		]
	}).pretty()
	
Combinando OR com AND: alunos cujo curso seja "Moda" ou "Engenharia Civil" E nome seja "João"

	> db.alunos.find({
		$or : [
			{"curso.nome" : "Moda"},
			{"curso.nome" : "Engenharia Civil"}
		],
		nome : "João"
	}).pretty()
	
Consulta com IN no lugar do OR

	> db.alunos.find({ 
		"curso.nome" : { 
			$in: [ "Moda", "Engenharia Civil" ] } 
	})
	
	
**Desvantagem db.alunos.find**

  - Traz o documento inteiro. Se fosse em um banco de dados relacional, poderia selecionar apenas alguns campos no select
  
**Documentação db.collection.find**

[db.collection.find()](https://docs.mongodb.com/manual/reference/method/db.collection.find)

**Operadores**

[Operadores](https://docs.mongodb.com/manual/reference/operator/)

[https://docs.mongodb.com/manual/reference/operator/query/or/](Operador OR)

[https://docs.mongodb.com/manual/reference/operator/query/in/](Operador IN)

[https://docs.mongodb.com/manual/reference/operator/query/and/](Operador AND)

[https://docs.mongodb.com/manual/reference/operator/query/not/](Operador NOT)