# elasticsearch-101

**ElasticSearch 101** est un workshop permettant de découvrir la base de données NoSQL ElasticSearch et son écosystème, étape par étape.

<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/"><img alt="Creative Commons Licence" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png" /></a>

Lors de la mise en oeuvre de ce workshop, n'hésitez pas à vous appuyer sur la [documentation officielle de ElasticSearch](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html)

## Installation

### Java
ElasticSearch étant basé sur le langage Java, veillez à disposer de **Java 8** installé sur votre machine. Vous pouvez vérifier l'installation de Java à l'aide de la commande `java -version`.

### ElasticSearch

Téléchargez la dernière version d'ElasticSearch sur [www.elastic.co](https://www.elastic.co/downloads/elasticsearch), ce workshop est compatible avec la version **5.0.0**.

Dézippez l'archive dans le dossier de votre choix, par exemple `~/progz/elasticsearch-5.0.0`.

Les exécutables nécessaires au fonctionnement d'ElasticSearch se trouvent dans le dossier `$HOME/progz/elasticsearch-<version>/bin`, **elasticsearch** permet de lancer le noeud et **plugin** permet d'installer des plugins.

Le fichier `$HOME/progz/elasticsearch-<version>/config/elasticsearch.yml`, au format [YAML](http://fr.wikipedia.org/wiki/YAML), permet de configurer ElasticSearch.

La configuration par défaut nous suffit pour l'instant.

Vous pouvez à présent démarrer le serveur.

	bin/elasticsearch

Il est possible d'ajouter des options Java pour augmenter la mémoire allouée à ElasticSearch en passant directement les paramètres de la JVM à l'exécutable ElsasticSearch.

	elasticsearch -Xmx=2G -Xms=2G

Pour vérifier le démarrage de votre noeud ElasticSearch : [http://localhost:9200/](http://localhost:9200/)

Vous devriez obtenir une réponse qui ressemble à celle là :
```javascript
{
    "name": "0yON8Wi",
    "cluster_name": "elasticsearch",
    "cluster_uuid": "zOMlo62SSDObhNxnlUx6Jw",
    "version": {
        "number": "5.0.0",
        "build_hash": "253032b",
        "build_date": "2016-10-26T04:37:51.531Z",
        "build_snapshot": false,
        "lucene_version": "6.2.0"
    },
    "tagline": "You Know, for Search"
}
```

### Kibana

Téléchargez la dernière version de Kibana correspondante à votre OS sur [www.elastic.co](https://www.elastic.co/products/kibana), ce workshop est compatible avec sur la version **5.0.0**.

Dézippez l'archive dans le dossier de votre choix, par exemple `~/progz/kibana-5.0.0`.

Ouvrez le fichier `config/kibana.yml` modifier les valeur suivante :

* elasticsearch.url: "http://localhost:9200" # l'instance d'ElasticSearch ciblée
* kibana.index: ".kibana" # dans le cadre du workshop, donner un nom unique

Pour vérifier l'installation de Kibana, vous pouvez lancer la commande suivante :

    bin/kibana --version

Vous pouvez maintenant lancer Kibana :

    bin/kibana

Connectez-vous à votre instance de Kibana locale avec votre browser  :

    http://localhost:5601

### Logstash

Télécharger la dernière version de logstash sur [www.elastic.co](https://www.elastic.co/downloads/logstash), ce workshop est compatible avec la version **5.0.0**.

Dézippez l'archive dans le dossier de votre choix, par exemple `~/progz/logstash-5.0.0`.

Pour vérifier l'installation de logstash, lancez la commande suivante :

    bin/logstash -V

## Concepts essentiels
Petit rappel des concepts essentiels...

### Document
Un document est un élément unitaire, au format JSON, stocké dans ElasticSearch.

### Noeud
Un noeud est une instance d'ElasticSearch, une noeud appartient à un **cluster**.

### Cluster
Un cluster est composé d'un ou plusieurs **noeuds** ElasticSearch qui sont connectés entre eux et qui partagent le même nom. Un cluster comporte un noeud maître unique (**master node**). En cas de défaillance du master node, un nouveau master node est élu parmis les noeuds restants.

### Index
Un index est un regroupement logique d'un ensemble de documents. Un index est composé de **shards**. Tous les documents appartienent à un index.

### Type
Un type est un sous-ensemble d'un index qui permet de regrouper des documents. De la même manière que pour les index, les types permettent de configurer le stockage des documents. Tout document appartient à un type.

### Shard
Un shard est un fragment d'un index. Ce sont les shards qui permettent de partitionner les index sur plusieurs noeuds. Ainsi, un index peut être partitionner sur autant de noeuds que cet index comporte de shards. Le nombre de shards par défaut est de **5**.

### Réplique
Une réplique est une copie intégrale des données d'un index. Les répliques permettent d'améliorer la tolérance à la panne du cluster ainsi que la durabilité des données. Une réplique comporte autant de shards que l'index original. Le nombre de répliques par défaut est de **1**.

## Prise en main de l'API

ElasticSearch expose l'ensemble de ses APIs à l'aide d'un API [REST](http://www.pompage.net/traduction/comment-j-ai-explique-rest-a-ma-femme), il est donc possible d'utiliser n'importe quel client HTTP pour manipuler ElasticSearch.

Le plugin **Sense** que vous avez peu-être installé facilite l'utilisation d'ElasticSearch.

Votre client *REST* préféré, si vous en avez un, fera sans problèmes l'affaire :D.

### Les convetions
L'API d'ElasticSearch est composé d'un ensemble d'APIs qui exposent des opérations spécialisées. Cette API est conforme aux standards REST. Elle est orientée ressources, s'appuie sur les verbes HTTP, les codes de retours HTTP...

D'une manière générale, les end points sont construits de la manière suivante :

```
	http://[host]:[port]/index/type/_action|id
```

Par exemple :
```
	localhost:9200/heroes/person/_search // pour effectuer une recherche parmi les documents de type 'person' dans l'index 'heroes'
	localhost:9200/heroes/person/_count // pour compter le nombre de documents de type 'person' dans l'index 'heroes'
	localhost:9200/heroes/person/ironman // pour accéder au document 'ironman'
```

Les actions sur les index permettent généralement d'effectuer l'opération sur plusieurs index simultanément. Par exemple, pour effectuer une requête sur les index index1 et index2, il est possible d'utiliser l'URL suivante :

```
	localhost:9200/heroes,vilains/_search
```

Vous trouverez de nombreux exemples (inclusions, exclusions, jokers, ...) dans la documentation.

## CRUD
### Insertion
Pour insérer un document, on utilise la requête suivante.

	curl -XPOST 'localhost:9200/heroes/person/ironman' -d '{
		"firstName" : "Tony",
		"lastName" : "Stark"
	}'

Le verbe, **POST**, indique qu'on insert un document
L'URL est construite de la manière suivante :

	<host>:<port>/<index>/<type>/<id>

Si l'index n'existe pas au moment de la création du document, celui-ci est créé automatiquement.

### PUT vs POST
Pour l'insertion de données, les verbes **POST** et **PUT** sont équivalents. Le verbe **POST** permet d'insérer des documents sans spéficier l'identifiant du document.

	curl -XPOST 'localhost:9200/heroes/person/' -d '{
		"firstName" : "Charles",
		"lastName" : "Xavier"
	}'

La réponse renvoyée contient l'identifiant généré par ElasticSearch.

```javascript
    {
      "_index": "heroes",
      "_type": "person",
      "_id": "AVLAbGUTL1N-EpXDlqsB",
      "_version": 1,
      "_shards": {
        "total": 2,
        "successful": 1,
        "failed": 0
      },
      "created": true
    }
```

### Extraction
Pour extraire un document à l'aide de son identifiant, on utilise la requête suivante.

	curl -XGET 'localhost:9200/heroes/person/ironman'


La réponse renvoyée est la suivante :

```javascript
	{
	   "_index": "heroes",
	   "_type": "person",
	   "_id": "ironman",
	   "_version": 11,
	   "found": true,
	   "_source": {
	      "firstName": "Tony",
	      "lastName": "Stark"
	   }
	}
```

L'attribut `found` indique que le document a bien été trouvé (`true` dans notre cas, `false` si le document n'a pas été trouvé). L'attribut `_source` contient le document extrait.

### Mise à jour
Pour mettre à jour les données, il est possible d'utiliser les requêtes **PUT** et **POST** présentées ci-dessus. Cette méthode permet de mettre à jour l'ensemble du document.

Il est possible d'effectuer des mises à jour partielles en utilisant l'API `_update`.

```
	curl -XPOST 'localhost:9200/heroes/person/ironman/_update' -d '{
		"doc" : {
			"firstName" : "Tomy"
		}
	}'
```

### Suppression
Pour supprimer un document, on utilise le verbe **DELETE**

	curl -XDELETE 'localhost:9200/heroes/person/ironman'

### Exists
Il est possible, à l'aide du verbe **HEAD** de vérifier l'existance d'un document (**attention, cette requête pas sous Sense!**)

	curl -XHEAD 'localhost:9200/heroes/person/ironman' -i

Les statut renvoyé :

* **200** indique que le document existe
* **404** indique que le document n'existe pas


## API de Recherche
Commençons par insérer quelques données ...

	curl -XPOST 'http://localhost:9200/heroes/person/ironman' -d '{"firstName":"Tony","lastName":"Stark","aka":"Iron Man","team":"Avengers","age":45}'
	curl -XPOST 'http://localhost:9200/heroes/person/thor' -d '{"firstName":"Thor","lastName":"Odinson","aka":"Thor","team":"Avengers","age":27}'
	curl -XPOST 'http://localhost:9200/heroes/person/antman' -d '{"firstName":"Hank","lastName":"Pym","aka":"Ant-Man","team":"Avengers","age":41}'
	curl -XPOST 'http://localhost:9200/heroes/person/wasp' -d '{"firstName":"Janet","lastName":"van Dyne","aka":"Wasp","team":"Avengers","age":32}'
	curl -XPOST 'http://localhost:9200/heroes/person/hulk' -d '{"firstName":"Bruce","lastName":"Banner","aka":"Hulk","team":"Avengers","age":41}'
	curl -XPOST 'http://localhost:9200/heroes/person/misterfantastic' -d '{"firstName":"Reed","lastName":"Richards","aka":"Mister Fantastic","team":"FantasticFour","age":47}'
	curl -XPOST 'http://localhost:9200/heroes/person/invisiblewoman' -d '{"firstName":"Susan","lastName":"Storm","aka":"Invisible Woman","team":"FantasticFour","age":29}'
	curl -XPOST 'http://localhost:9200/heroes/person/thehumantorch' -d '{"firstName":"Johnny","lastName":"Storm","aka":"The Human Torch","team":"FantasticFour","age":25}'
	curl -XPOST 'http://localhost:9200/heroes/person/thething' -d '{"firstName":"Ben","lastName":"Grimm","aka":"The Thing","team":"FantasticFour","age":42}'


L'API `_search` permet d'effectuer des [recherches](https://www.elastic.co/guide/en/elasticsearch/reference/current/search.html) dans ElasticSearch.

### Recherche
La requête suivante lance une recherche sur l'ensemble des documents de type `person` dans l'index `heroes` (par défault, une recherche remonte 10 résultats) :

	curl -XPOST 'http://localhost:9200/heroes/person/_search'

La requête suivante permet de rechercher tous ls documents qui ont un attribut `lastName` dont la valeur est `storm` :

	curl -XPOST 'http://localhost:9200/heroes/person/_search' -d '{
	    "query": {
	        "match": {
	           "lastName": "storm"
	        }
	    }
	}'


La requête suivante permet d'effectuer une recherche sur les documents dont le prénom commence par un `t` :

	curl -XPOST 'http://localhost:9200/heroes/person/_search' -d '{
	    "query": {
	        "wildcard": {
	           "firstName": {
	              "value": "t*"
	           }
	        }
	    }
	}

Il est possible de faire des recherches à plusieurs niveaux :

* sur un type et un index donnés : `localhost:9200/heroes/person/_search`
* sur l'ensemble des types d'un index donné : `localhost:9200/heroes/_search`
* sur l'ensemble des index d'un cluster : `localhost:9200/_search`

Effectuez maintenant quelques recherches à l'aide de requêtes de type [Query String Query](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-query-string-query.html).

### Agrégations
Les [agrégations](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations.html) permettent de regrouper les données et d'effectuer des calculs sur les documents contenus dans les index.

Bien qu'il soit possible de combiner recherche et aggrégations, nous ne nous intéressons pas ici aux recherches (d'où l'attribut `"size": 0` ...).

Obtenir la répartition des valeurs du terme `team` dans les documents de type `person` de l'index `heroes` :

```
	curl -XPOST 'http://localhost:9200/heroes/person/_search' -d '{
	    "size": 0,
	    "aggs" : {
	        "teams" : {
	            "terms": {
	                "field": "team.keyword"
	            }
	        }
	    }
	}
```

Il est possible de faire des sous-agrégations. Obtenir la répartition des valeurs du terme `lastName` dans la répartion du terme `team` dans les documents de type `person` de l'index `heroes` :

	curl -XPOST 'http://localhost:9200/heroes/person/_search' -d '{
	    "size": 0,
	    "aggs" : {
	        "teams" : {
	            "terms": {
	                "field": "team.keyword"
	            },
				"aggs" : {
	                "names" : {
	                    "terms": {
	    	                "field": "lastName.keyword"
	    	            }
	                }
				}
	        }
	    }
	}

Il est possible de faire des calculs avec les aggrégations. L'âge moyen des membres de chaque équipe :

	curl -XPOST 'http://localhost:9200/heroes/person/_search' -d '{
	    "size": 0,
	     "aggs" : {
	         "teams" : {
	            "terms": {
	                "field": "team.keyword",
	                "order" : { "avgAge" : "desc" }
	            },
				"aggs" : {
	                "avgAge" : {
	                    "avg" : {
	                        "field" : "age"
	                    }
	                }
				}
	        }
	    }
	}

Quelques exercices complémentaires :

* Vous pouvez à présent écrire une agrégation qui calcule l'age maximum par équipe.
* Vous pouvez à présent écrire une agrégation qui calcule l'age maximum par équipe pour les personnages dont le nom commence par la lettre 't'.
* En utilisant les agrégations de type [Histogram](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations-bucket-histogram-aggregation.html), créez une aggrégation permettant de regrouper les héros par tranche d'age (par dizaine).
