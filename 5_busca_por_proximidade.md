## Endereços e posicionamentos

Converte um endereço em coordenadas [latitude, longitude]

	[Latitude and Longitude Finder](http://www.latlong.net)
	
## Posicionamento no MongoDB

	> db.alunos.find()
	
	> db.alunos.find({nome : "João"})
	
Salvar a localização do aluno "João"	

	> db.alunos.update(
	{ "_id" : ObjectId("5f1806678c7a731623ad4d48") },
	{
		$set : {
			localizacao : {
				endereco : "Rua Vergueiro, 3185",
				cidade : "São Paulo",
				coordinates : [-23.588213, -46.632356],
				type : "Point"
				}
			}
	})
	
### Representar um ponto geográfico no MongoDB	

**coordinates** é o nome PADRÃO	para armazenar uma [latitude, longitude] e é necessário informar o tipo de coordenada (no caso, **Point**)

O restante é opcional (cidade, país, etc...)

### Importar as coordenadas geográficas de um arquivo json para a coleção alunos

[alunos.json](/alunos.json)

	$ cd pasta/onde/esta/arquivo.json
	
	// jsonArray pois o arquivo contém um array:
	   [ {"nome" : "Guilherme", "localizacao" : { "type" : "Point", "coordinates" : [-23.5882133, -46.63235580000003]}},... ]

	> mongoimport -c alunos --jsonArray < alunos.json	
	19 document(s) imported successfully. 0 document(s) failed to import.
	
	> db.alunos.find()


## Buscando por proximidade

Criar um índice de busca (campo "localizacao") que tem estrutura de "ponto", com latitude e longitude


	"localizacao" será indexada para uma busca em esfera 2d, contém 2 dimensões (lat, long) - isso é, sem a altitude	
	> db.alunos.createIndex({
		localizacao : "2dsphere"
	})


Realizar a pesquisa de proximidade geográfica: agregar (**aggregate**) os dados mais próximos e devolver o resultado

	- Tipo de busca: procurar por proximidade ($geoNear)
	
	- Indicar o que está próximo da coordenada: near
	
	- Indicar como calcular a distância entre dois pontos: a comparação não deve ser entre 
		as distâncias de uma linha, e sim, de uma esfera (spherical : true)

	- Criar o campo distanceField : "distance.calculada" onde será armazenada a distância entre as coordenadas
	
	db.alunos.aggregate([
	{
		$geoNear : {
			near : {
				coordinates: [-23.5640265, -46.6527128],
				type : "Point"
			},
			distanceField : "distancia.calculada",
			spherical : true
		}
	}
	])


	Output
	
	{ "_id" : ObjectId("5f18c9b449b03b0f4d6d92bc"), "nome" : "Marcelo", "localizacao" : { "type" : "Point", "coordinates" : [ -23.5640265, -46.6527128 ] }, "distancia" : { "calculada" : 0 } }
	
	{ "_id" : ObjectId("5f18c9b449b03b0f4d6d92bd"), "nome" : "Sofia", "localizacao" : { "type" : "Point", "coordinates" : [ -23.5673307, -46.6529703 ] }, "distancia" : { "calculada" : 254.0997430407364 } }
	
	{ "_id" : ObjectId("5f18c9b449b03b0f4d6d92b3"), "nome" : "Stella", "localizacao" : { "type" : "Point", "coordinates" : [ -23.5623743, -46.6478634 ] }, "distancia" : { "calculada" : 554.3966936218583 } }
	
	...diversos outros alunos com as respectivas distâncias calculadas...
	
O primeiro é o próprio ponto pesquisado	[ -23.5640265, -46.6527128 ] com distancia.calculada = 0

Depois o segundo mais próximo e assim por diante


Eliminar o primeiro documento

	Traz 4 documentos (num : 4) e "pula" 1 ($skip :1)
	
	db.alunos.aggregate([
	{
		$geoNear : {
			near : {
				coordinates: [-23.5640265, -46.6527128],
				type : "Point"
			},
			distanceField : "distancia.calculada",
			spherical : true,
			num : 4
		}
	},
	{ $skip :1 }
	])
	
	"$geoNear no longer supports the 'num' parameter. Use a $limit stage instead."
	

De acordo com a documentação do MongoBD:

"*Starting in version 4.2, MongoDB removes the limit and num options for the $geoNear stage as well as the default limit of 100 documents. To limit the results of $geoNear, use the $geoNear stage with the $limit stage.*"

Fica assim:
	
	db.alunos.aggregate([
	{
		$geoNear : {
			near : {
				coordinates: [-23.5640265, -46.6527128],
				type : "Point"
			},
			distanceField : "distancia.calculada",
			spherical : true
			
		}
	},
	{ $limit: 4 }
	])
	
	
[Documentação $geoNear](https://docs.mongodb.com/manual/reference/operator/aggregation/geoNear)	