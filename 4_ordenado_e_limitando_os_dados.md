## Buscando e limitando registros

Busca em todos os arrays de notas (isso é, de TODOS alunos) quem tenham uma nota = 8.5

 	Faz a busca nos arrays
	> db.alunos.find(
    {
    	"notas" : 8.5
	})
	
	Output
	{ "_id" : ObjectId("5f1806678c7a731623ad4d48"), "nome" : "João", "data_nascimento" : ISODate("1989-04-24T03:00:00Z"), "curso" : { "nome" : "Engenharia Civil" }, "notas" : [ 10, 9.6, 4.7, 8.5, 8.5, 8.5, 3, 8.5, 3, 8.5, 3 ], "habilidades" : [ { "nome" : "Inglês", "nivel" : "avançado" }, { "nome" : "Espanhol", "nivel" : "básico" } ] }


Busca em todos os arrays de notas (isso é, de TODOS alunos) quem tenham uma nota > 5

	find: traz todos registros
	db.alunos.find({
		notas : { $gt : 5 }
	})
	
	Output
	{ "_id" : ObjectId("5f1806678c7a731623ad4d48"), "nome" : "João", "data_nascimento" : ISODate("1989-04-24T03:00:00Z"), "curso" : { "nome" : "Engenharia Civil" }, "notas" 	: [ 10, 9.6, 4.7, 8.5, 8.5, 8.5, 3, 8.5, 3, 8.5, 3 ], "habilidades" : [ { "nome" : "Inglês", "nivel" : "avançado" }, { "nome" : "Espanhol", "nivel" : "básico" } ] }
	
	{ "_id" : ObjectId("5f18661a8c7a731623ad4d4d"), "nome" : "Fernando", "data_nascimento" : ISODate("1994-04-26T03:00:00Z"), "notas" : [ 10, 4.5, 7 ], "curso" : { "nome" : "Sistemas de Informação" } }
	

Inserindo mais alguns alunos

	> db.alunos.insert({
		nome : "André",
		data_nascimento : new Date(1991,01,25),
		curso : {
			nome : "Matemática"
			},
			notas : [ 7, 5, 9, 4.5 ]
	})

	> db.alunos.insert({
		nome : "Lúcia",
		data_nascimento : new Date(1984,07,17),
		curso : {
			nome : "Matemática"
			},
			notas : [ 8, 9.5,  10 ]
	})
	
	
Busca apenas UM aluno que tem nota > 5 

	findOne: traz apenas um registro (o primeiro registro)
	db.alunos.findOne({
		notas : { $gt : 5}
	})	
	
	Output
	
		{
		"_id" : ObjectId("5f1806678c7a731623ad4d48"),
		"nome" : "João",
		"data_nascimento" : ISODate("1989-04-24T03:00:00Z"),
		"curso" : {
			"nome" : "Engenharia Civil"
		},
		"notas" : [
			10,
			9.6,
			4.7,
			8.5,
			8.5,
			8.5,
			3,
			8.5,
			3,
			8.5,
			3
		],
		"habilidades" : [
			{
				"nome" : "Inglês",
				"nivel" : "avançado"
			},
			{
				"nome" : "Espanhol",
				"nivel" : "básico"
			}
		]
	}
	
Buscando alunos com nota < 5

	> db.alunos.find({"notas":{$lt:5}})
	
	Output
	
	{ "_id" : ObjectId("5f1806678c7a731623ad4d48"), "nome" : "João", "data_nascimento" : ISODate("1989-04-24T03:00:00Z"), "curso" : { "nome" : "Engenharia Civil" }, "notas" : [ 10, 9.6, 4.7, 8.5, 8.5, 8.5, 3, 8.5, 3, 8.5, 3 ], "habilidades" : [ { "nome" : "Inglês", "nivel" : "avançado" }, { "nome" : "Espanhol", "nivel" : "básico" } ] }
	
	{ "_id" : ObjectId("5f18661a8c7a731623ad4d4d"), "nome" : "Fernando", "data_nascimento" : ISODate("1994-04-26T03:00:00Z"), "notas" : [ 10, 4.5, 7 ], "curso" : { "nome" : "Sistemas de Informação" } }
	
	{ "_id" : ObjectId("5f18b8b82c623c7556b85fd3"), "nome" : "André", "data_nascimento" : ISODate("1991-02-25T03:00:00Z"), "curso" : { "nome" : "Matemática" }, "notas" : [ 7, 5, 9, 4.5 ] }
	
	
## Ordenando registros

Ordenar os alunos pelo "nome" em ordem CRESCENTE (1)

	> db.alunos.find().sort({"nome" : 1})
	
Ordenar os alunos pelo "nome" em ordem DECRESCENTE (-1)	

	> db.alunos.find().sort({"nome" : -1})	
	
Ordenar os alunos pelo "nome" em ordem CRESCENTE (1) e LIMITANDO a 3 documentos

	> db.alunos.find().sort({"nome" : 1}).limit(3)
	
	
[Query and Projection Operators](https://docs.mongodb.com/manual/reference/operator/query)	