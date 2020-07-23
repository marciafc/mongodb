## Atualização completa e parcial de documentos

	> db.alunos.insert({
		nome : "Fernando",
		data_nascimento : new Date(1994, 03, 26),
		notas : [ 10, 4.5, 7],
		curso : {
			nome : "Sistema de informação"
		}
	})
	
	
Atualizando "Sistema de informação" para "Sistemas de informação"

	Equivalente UPDATE cursos SET nome = "Sistemas de informação" WHERE nome = "Sistema de informação"
	> db.alunos.update(
    	{"curso.nome" : "Sistema de informação"},
    	{
        	$set : {
            	"curso.nome" : "Sistemas de informação"
        	}
    	}    
	)
	
Cuidado ao dar upadate, pois a query abaixo, atualizaria o DOCUMENTO INTEIRO, não apenas o nome do curso! Usar $set (vide acima)

	> db.alunos.update(
    	{"curso.nome" : "Sistema de informação"}
    	{ 
        	"nome" : "Sistemas de informação"
    	}
	)
	
Atenção: UPDATE por default, só é executado para o PRIMEIRO DOCUMENTO encontrado	

	Atualizando nome do curso Engenharia Civil para Eng. Civil	
	> db.alunos.update(
    	{"curso.nome" : "Engenharia Civil"},
    	{
        	$set : {
            	"curso.nome" : "Eng. Civil"
        	}
    	}
	)
	
	> db.alunos.update(
    	{"curso.nome" : "Engenharia Civil"},
    	{
        	$set : {
            	"curso.nome" : "Eng. Civil"
        	}
    	}
	)
	
Para realizar o UPDATE para MÚLTIPLOS DOCUMENTOS, necessário usar multi : true 

	> db.alunos.update(
    	{"curso.nome" : "Eng. Civil"},
    	{
        	$set : {
            	"curso.nome" : "Engenharia Civil"
        	}
    	}, 
       {
         multi : true 
       }
	)
	
## Mais operações e cuidados

Adicionar elemento no array

	$push: Permite duplicidade
	> db.alunos.update(
		{"_id" : ObjectId("5f1806678c7a731623ad4d48")},
		{
			$push : {
				notas : 8.5
			}    
		}
	)
	
	> db.alunos.find().pretty()


Adicionar elemento no array, se ainda não existir

	$addToSet: Não permite duplicidade
	> db.alunos.update(
			{"_id" : ObjectId("5f1806678c7a731623ad4d48")},
			{
				$addToSet : {
					notas : 8.5
				}    
			}
		)


Atualiza o array de notas com mais de um elemento

	> db.alunos.update(
    	{"_id" : ObjectId("5f1806678c7a731623ad4d48")},
    	{
        	$push : {
            	"notas" : {$each : [8.5, 3] }
        	}
    	}
	)	
	
Cuidado: isso irá inserir um array dentro de outro array, ao invés de apenas os valores	

	db.alunos.update(
    > {"_id" : ObjectId("5f1806678c7a731623ad4d48")},
    	{
        	$push : {
            	notas : [8.5, 3]
        	}    
   	 	}
	)
	

	
[Update Operators](https://docs.mongodb.com/manual/reference/operator/update)

[Array Update Operators](https://docs.mongodb.com/manual/reference/operator/update-array/)

[Inserir um valor dentro de um array - $push](https://docs.mongodb.com/manual/reference/operator/update/push/#up._S_push)

[Inserir um elemento, caso ele ainda não exista](https://docs.mongodb.com/manual/reference/operator/update/addToSet/#up._S_addToSet)