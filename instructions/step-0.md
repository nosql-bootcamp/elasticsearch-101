## Installation

### Utilisation d'une version cloud

Plutôt que d'installer ElasticSearch localement, vous pouvez profiter de l'essai gratuit (15 jours) de la version cloud qui est accessible sur [https://www.elastic.co/fr/](https://www.elastic.co/fr/) (bouton bleu `Essai gratuit` en haut à droite). 

**Pensez à bien enregistrer votre mot de passe, il ne vous est donné qu'une seule fois.**

Cette option est plus simple, néanmoins, vous devrez adapter les requêtes dans le reste de ce document, par exemple : 

```
curl -XGET 'http://localhost:9200/'
```
deviendrait :

```
curl -XGET -u elastic:<password> https://<deployment-port>:<deployment-url>
```

### Java

ElasticSearch étant basé sur le langage Java, veillez à disposer de **Java 8** installé sur votre machine. Vous pouvez vérifier l'installation de Java à l'aide de la commande `java -version`.

### ElasticSearch

Téléchargez la dernière version d'ElasticSearch sur [www.elastic.co](https://www.elastic.co/downloads/elasticsearch), ce workshop est compatible avec la version **7.10.x**.

Dézippez l'archive dans le dossier de votre choix, par exemple `~/progs/elasticsearch-7.10.1`.

Les exécutables nécessaires au fonctionnement d'ElasticSearch se trouvent dans le dossier `$HOME/progs/elasticsearch-7.10.1/bin`, **elasticsearch** permet de lancer le noeud et **plugin** permet d'installer des plugins.

Le fichier `$HOME/progs/elasticsearch-7.10.1/config/elasticsearch.yml`, au format [YAML](http://fr.wikipedia.org/wiki/YAML), permet de configurer ElasticSearch.

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
```json
{
  "name" : "MacBook-Pro",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "iVKOkmV9RYKWEMzr4_J-1w",
  "version" : {
    "number" : "7.10.1",
    "build_flavor" : "default",
    "build_type" : "tar",
    "build_hash" : "1c34507e66d7db1211f66f3513706fdf548736aa",
    "build_date" : "2020-12-05T01:00:33.671820Z",
    "build_snapshot" : false,
    "lucene_version" : "8.7.0",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
```

### Kibana

Téléchargez la dernière version de Kibana correspondante à votre OS sur [www.elastic.co](https://www.elastic.co/downloads/kibana), ce workshop est compatible avec sur la version **7.10.x**.

Dézippez l'archive dans le dossier de votre choix, par exemple `~/progs/kibana-7.10.1`.

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
