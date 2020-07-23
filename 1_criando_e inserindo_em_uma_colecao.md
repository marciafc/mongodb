## Preparar ambiente

[Download MongoDB Community Server](https://www.mongodb.com/try/download/community) | [Manual instalação](https://docs.mongodb.com/manual/administration/install-community/)

[Manual](https://docs.mongodb.com/manual/)

**Variável de ambiente PATH**

Configurar caso NÃO esteja no diretório default (/usr/bin/mongo)

	// Onde está instalado o Mongo
	$ locate -b '\mongo'

	$ cd
	$ vi .profile
	
	Incluir no final do arquivo:
	export PATH=$PATH:path\ate\o\bin

## Inicializar MongoDB

[Inicializar](https://docs.mongodb.com/manual/tutorial/manage-mongodb-processes/)
	
	// Servidor
	$ mongod --dbpath path\onde\inicializara\mongodb	
	OU
	Criar o diretório /data/db e dar permissão de acesso ao usuário e então executar o comando mongod 
	$ mongod
	
	// Client  (em outro terminal)
	$ mongo	
	
## Criando coleções e registros

No terminal do client

Criar a coleção

	> db.createCollection("alunos")
	
Inserir aluno

	> db.alunos.insert(
		{
		"nome": "João",
		"data_nascimento": new Date(1989, 03, 24)
		}
	)	
	
	> db.alunos.insert(
		{
		"nome": "João",
		"data_nascimento": new Date(1989, 03, 24),
		"curso": {"nome": "Engenharia Civil"},
		"notas": [10, 9.6, 4.7],
		"habilidades": [{"nome": "Inglês", "nivel": "avançado"}, {"nome": "Espanhol", "nivel": "básico"}]
		}
	)
	
Listar todos alunos

	> db.alunos.find()
	
Remover aluno por id

	> db.alunos.remove({
	  "_id" : ObjectId("5f1804168c7a731623ad4d47")
	})
	
Inserindo mais alguns alunos

	> db.alunos.insert(
		{
		"nome": "Mario",
		"data_nascimento": new Date(1971, 01, 9),
		"curso": {"nome": "Engenharia Civil"},		
		"habilidades": [{"nome": "Inglês", "nivel": "avançado"}, {"nome": "Espanhol", "nivel": "básico"}]
		}
	)	
	
Aspas no nome da chave é opcional (recomendado usar as aspas para evitar problema com algum caracter não suportado)

	> db.alunos.insert(
		{
		nome: "Pedro",
		data_nascimento: new Date(1990, 04, 10),
		curso: {nome: "Engenharia Química"},		
		habilidades: [{nome: "Inglês", nivel: "básico"}, {nome: "Espanhol", nivel: "básico"}]
		}
	)
	
	> db.alunos.insert(
		{
		nome: "Maria",
		data_nascimento: new Date(1977, 12, 23),
		curso: {nome: "Moda"},		
		habilidades: [{nome: "Alemão", nivel: "básico"}]
		}
	)
	
	
## MongoDB: Vantagem X Desvantagem

(+) Flexibilidade e liberdade na criação da estrutura

(+) Realizar buscas por proximidade

(-) Necessidade de realizar diversas operações de agregação em uma única query (é custoso para o MongoDB)