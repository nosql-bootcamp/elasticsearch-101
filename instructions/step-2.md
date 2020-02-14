## API de Recherche

Commençons par insérer quelques données ...

```bash
curl -XPOST 'http://localhost:9200/heroes/_doc/ironman' -H 'Content-Type: application/json' -d '{"firstName":"Tony","lastName":"Stark","aka":"Iron Man","team":"Avengers","age":45}'
curl -XPOST 'http://localhost:9200/heroes/_doc/thor' -H 'Content-Type: application/json' -d '{"firstName":"Thor","lastName":"Odinson","aka":"Thor","team":"Avengers","age":27}'
curl -XPOST 'http://localhost:9200/heroes/_doc/antman' -H 'Content-Type: application/json' -d '{"firstName":"Hank","lastName":"Pym","aka":"Ant-Man","team":"Avengers","age":41}'
curl -XPOST 'http://localhost:9200/heroes/_doc/wasp' -H 'Content-Type: application/json' -d '{"firstName":"Janet","lastName":"van Dyne","aka":"Wasp","team":"Avengers","age":32}'
curl -XPOST 'http://localhost:9200/heroes/_doc/hulk' -H 'Content-Type: application/json' -d '{"firstName":"Bruce","lastName":"Banner","aka":"Hulk","team":"Avengers","age":41}'
curl -XPOST 'http://localhost:9200/heroes/_doc/misterfantastic' -H 'Content-Type: application/json' -d '{"firstName":"Reed","lastName":"Richards","aka":"Mister Fantastic","team":"FantasticFour","age":47}'
curl -XPOST 'http://localhost:9200/heroes/_doc/invisiblewoman' -H 'Content-Type: application/json' -d '{"firstName":"Susan","lastName":"Storm","aka":"Invisible Woman","team":"FantasticFour","age":29}'
curl -XPOST 'http://localhost:9200/heroes/_doc/thehumantorch' -H 'Content-Type: application/json' -d '{"firstName":"Johnny","lastName":"Storm","aka":"The Human Torch","team":"FantasticFour","age":25}'
curl -XPOST 'http://localhost:9200/heroes/_doc/thething' -H 'Content-Type: application/json' -d '{"firstName":"Ben","lastName":"Grimm","aka":"The Thing","team":"FantasticFour","age":42}'
```

L'API `_search` permet d'effectuer des [recherches](https://www.elastic.co/guide/en/elasticsearch/reference/current/search.html) dans ElasticSearch.

### Recherche

La requête suivante lance une recherche sur l'ensemble des documents de l'index `heroes` (par défault, une recherche remonte 10 résultats) :

```bash
curl -XPOST 'http://localhost:9200/heroes/_search'
```

Il est possible de formatter correctement le résultat de la recherche grâce au paramètre `pretty`.

```bash
curl -XPOST 'http://localhost:9200/heroes/_search?pretty'
```

La requête suivante permet de rechercher tous les documents qui ont un attribut `lastName` dont la valeur est `storm` :

```bash
curl -XPOST 'http://localhost:9200/heroes/_search' -H 'Content-Type: application/json' -d '{
    "query": {
        "match": {
           "lastName": "storm"
        }
    }
}'
```

La requête suivante permet d'effectuer une recherche sur les documents dont le prénom commence par un `t` :

```bash
curl -XPOST 'http://localhost:9200/heroes/_search' -H 'Content-Type: application/json' -d '{
    "query": {
        "wildcard": {
           "firstName": {
              "value": "t*"
           }
        }
    }
}'
```

Il est possible de faire des recherches à plusieurs niveaux :

* sur un index donné : `http://localhost:9200/heroes/_search`
* sur plusieurs index : `http://localhost:9200/heroes,vilains/_search`
* sur l'ensemble des index d'un cluster : `http://localhost:9200/_search`

Effectuez maintenant quelques recherches à l'aide de requêtes de type [Query String Query](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-query-string-query.html).

### Agrégations

Les [agrégations](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations.html) permettent de regrouper les données et d'effectuer des calculs sur les documents contenus dans les index.

Bien qu'il soit possible de combiner recherche et agrégations, nous ne nous intéressons pas ici aux recherches (d'où l'attribut `"size": 0` ...).

Pour obtenir la répartition des valeurs du terme `team` dans les documents de l'index `heroes` :

```bash
curl -XPOST 'http://localhost:9200/heroes/_search' -H 'Content-Type: application/json' -d '{
    "size": 0,
    "aggs" : {
        "teams" : {
            "terms": {
                "field": "team.keyword"
            }
        }
    }
}'
```

Il est possible de faire des sous-agrégations. Pour obtenir la répartition des valeurs du terme `lastName` dans la répartition du terme `team` dans les documents de l'index `heroes` :

```bash
curl -XPOST 'http://localhost:9200/heroes/_search' -H 'Content-Type: application/json' -d '{
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
}'
```

Il est possible de faire des calculs avec les agrégations. Par exemple pour l'âge moyen des membres de chaque équipe :

```bash
curl -XPOST 'http://localhost:9200/heroes/_search' -H 'Content-Type: application/json' -d '{
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
}'
```

Quelques exercices complémentaires :

* Vous pouvez à présent écrire une agrégation qui calcule l'âge maximum par équipe.
* Vous pouvez à présent écrire une agrégation qui calcule l'âge maximum par équipe pour les personnages dont le nom commence par la lettre 't'.
* En utilisant les agrégations de type [Histogram](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations-bucket-histogram-aggregation.html), créez une agrégation permettant de regrouper les héros par tranche d'âge (par dizaine).
