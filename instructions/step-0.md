## Installation

### Java

ElasticSearch étant basé sur le langage Java, veillez à disposer de **Java 8** installé sur votre machine. Vous pouvez vérifier l'installation de Java à l'aide de la commande `java -version`.

### ElasticSearch

Téléchargez la dernière version d'ElasticSearch sur [www.elastic.co](https://www.elastic.co/downloads/elasticsearch), ce workshop est compatible avec la version **6.6.0**.

Dézippez l'archive dans le dossier de votre choix, par exemple `~/progs/elasticsearch-6.6.0`.

Les exécutables nécessaires au fonctionnement d'ElasticSearch se trouvent dans le dossier `$HOME/progs/elasticsearch-<version>/bin`, **elasticsearch** permet de lancer le noeud et **plugin** permet d'installer des plugins.

Le fichier `$HOME/progs/elasticsearch-6.6.0/config/elasticsearch.yml`, au format [YAML](http://fr.wikipedia.org/wiki/YAML), permet de configurer ElasticSearch.

La configuration par défaut nous suffit pour l'instant.

Vous pouvez à présent démarrer le serveur.

```bash
bin/elasticsearch
```

Il est possible d'ajouter des options Java pour augmenter la mémoire allouée à ElasticSearch en passant directement les paramètres de la JVM à l'exécutable ElsasticSearch.

```bash
bin/elasticsearch -Xmx=2G -Xms=2G
```

Pour vérifier le démarrage de votre noeud ElasticSearch : [http://localhost:9200/](http://localhost:9200/)

Vous devriez obtenir une réponse qui ressemble à celle là :
```javascript
{
  "name" : "t2lmMD5",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "n3eGSgm8SYidkEOyTWQcnw",
  "version" : {
    "number" : "6.6.0",
    "build_flavor" : "default",
    "build_type" : "tar",
    "build_hash" : "a9861f4",
    "build_date" : "2019-01-24T11:27:09.439740Z",
    "build_snapshot" : false,
    "lucene_version" : "7.6.0",
    "minimum_wire_compatibility_version" : "5.6.0",
    "minimum_index_compatibility_version" : "5.0.0"
  },
  "tagline" : "You Know, for Search"
}
```

### Kibana

Téléchargez la dernière version de Kibana correspondante à votre OS sur [www.elastic.co](https://www.elastic.co/downloads/kibana), ce workshop est compatible avec sur la version **6.6.0**.

Dézippez l'archive dans le dossier de votre choix, par exemple `~/progs/kibana-6.6.0`.

Pour vérifier l'installation de Kibana, vous pouvez lancer la commande suivante :

```bash
bin/kibana --version
```

Vous pouvez maintenant lancer Kibana :

```bash
bin/kibana
```

Connectez-vous à votre instance de Kibana locale avec votre browser : [http://localhost:5601](http://localhost:5601)

## Next

Vous pouvez passer à l'étape suivante : [Opérations CRUD](./step-1.md)
