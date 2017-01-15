## Installation

### Java

ElasticSearch étant basé sur le langage Java, veillez à disposer de **Java 8** installé sur votre machine. Vous pouvez vérifier l'installation de Java à l'aide de la commande `java -version`.

### ElasticSearch

Téléchargez la dernière version d'ElasticSearch sur [www.elastic.co](https://www.elastic.co/downloads/elasticsearch), ce workshop est compatible avec la version **5.1.2**.

Dézippez l'archive dans le dossier de votre choix, par exemple `~/progs/elasticsearch-5.1.2`.

Les exécutables nécessaires au fonctionnement d'ElasticSearch se trouvent dans le dossier `$HOME/progs/elasticsearch-<version>/bin`, **elasticsearch** permet de lancer le noeud et **plugin** permet d'installer des plugins.

Le fichier `$HOME/progs/elasticsearch-5.1.2/config/elasticsearch.yml`, au format [YAML](http://fr.wikipedia.org/wiki/YAML), permet de configurer ElasticSearch.

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

Téléchargez la dernière version de Kibana correspondante à votre OS sur [www.elastic.co](https://www.elastic.co/downloads/kibana), ce workshop est compatible avec sur la version **5.1.2**.

Dézippez l'archive dans le dossier de votre choix, par exemple `~/progs/kibana-5.1.2`.

Pour vérifier l'installation de Kibana, vous pouvez lancer la commande suivante :

```bash
bin/kibana --version
```

Vous pouvez maintenant lancer Kibana :

```bash
bin/kibana
```

Connectez-vous à votre instance de Kibana locale avec votre browser : http://localhost:5601

## Next

Vous pouvez passer à l'étape suivante : [Opérations CRUD](./step-1.md)
