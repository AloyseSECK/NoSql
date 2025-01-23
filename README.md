# REDIS : (Une base de données NoSql)

## Qu'est ce qu'une base de donées NoSql ?
Une base de données NoSql est une base de données qui ne repose pas sur le modèle relationnel. Elle est conçue pour gérer des données non structurées ou semi-structurées. Les bases de données NoSql sont souvent utilisées pour stocker de grandes quantités de données non structurées, telles que des données de type clé-valeur, des documents, des graphiques, etc.

## Quelles sont les familles de bases de données NoSql ?
En général, les bases de données NoSql sont classées en quatre familles principales :

- **Les bases de données de type clé-valeur** : Ces bases de données stockent les données sous forme de paires clé-valeur. Elles sont très rapides et efficaces pour stocker des données simples. Exemple : Redis, DynamoDB, etc.

- **Les bases de données de type document** : Ces bases de données stockent les données sous forme de documents. Chaque document est une structure de données complexe qui peut contenir des champs, des sous-documents, etc. Exemple : MongoDB, CouchDB, etc.

- **Les bases de données de type colonne** : Ces bases de données stockent les données sous forme de colonnes plutôt que de lignes. Elles sont très efficaces pour les requêtes analytiques. Exemple : Cassandra, HBase, etc.

- **Les bases de données de type graphe** : Ces bases de données stockent les données sous forme de graphes. Elles sont très efficaces pour stocker des données interconnectées. Exemple : Neo4j, ArangoDB, etc.

## Qu'est ce que REDIS ? 

Redis est une base de données de type clé-valeur open-source. Elle est très rapide et efficace pour stocker des données simples. Redis est souvent utilisé pour stocker des données en cache, des sessions utilisateur, des données de session, etc.

## Utilisation de REDIS
Nous allons utiliser redis en mode conteneur avec docker.

### C'est quoi docker ?
Docker est un outil qui permet de créer, déployer et exécuter des applications en utilisant des conteneurs. Les conteneurs permettent de packager une application avec toutes les parties nécessaires, telles que les bibliothèques et autres dépendances, et de les expédier en tant que package.

De manière plus simple, Docker permet de créer des conteneurs qui sont des environnements isolés pour exécuter des applications. Ces conteneurs sont légers et portables, et peuvent être exécutés sur n'importe quel système qui exécute Docker. 

Pour utiliser docker vous pouvez suivre cette [documentation](https://docs.docker.com/engine/install/ubuntu/) en ligne pour l'installer sur ubuntu.

#### Installation de Redis avec docker 
Pour lancer un server redis, il suffit de taper la commande suivante sur votre terminal, une fois que docker est installé sur votre machine.

```bash
docker run --name redis-server -p 6379:6379 -d redis
```

**Explication de la commande :** La commande ci-dessus permet de lancer un conteneur redis en mode détaché avec le nom `redis-server` et le port `6379` exposé sur le port `6379` de la machine hôte. La définition des ports est importante car elle permet de faire communiquer le conteneur avec votre machine.

Pour vérifier que le conteneur est bien lancé, vous pouvez taper la commande suivante :

```bash
docker ps
```
Vous obtenez normalement un résultat similaire à celui-ci :

```bash
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                                       NAMES
21fec104de58   redis     "docker-entrypoint.s…"   9 minutes ago   Up 9 minutes   0.0.0.0:6379->6379/tcp, :::6379->6379/tcp   redis-server
```

Une fois que le serveur est lancé, vous pouvez vous connecter à redis en utilisant la commande suivante :

```bash
docker exec -it redis-server redis-cli
```


A partir de maintenant, vous pouvez interagir avec le serveur redis pour créer des clés, des valeurs, des listes, des ensembles, des hachages, etc. Nous verrons les différentes manipulations dans la partie suivante.

## Manipulation de REDIS

### Quelques commandes basiques
Pour commmencer, créons une clef  appelée `demo` et attribuons lui la valeur `"bonjour"` en utilisant la commande suivante :

```bash
set demo "bonjour"
```
Le serveur doit vous répondre par `OK` si la commande a été exécutée avec succès.

Faites la même chose pour créer une clef nommée `user:1234` et attribuez lui la valeur `"Samir"`. Vous pouvez vérifier la réponse ci dessous : 

<details close>
<summary>Réponse</summary>

<b> set user:1234 "Samir" </b>

</details>
<br >
Lorsqu'une clef a été créée, vous pouvez récupérer la valeur grâce à la commande `get` suivi du nom de la clef. Par exemple, pour récupérer la valeur de la clef `demo`, vous pouvez taper la commande suivante :

```bash
get demo
```

**Entrainement** : Récupérez la valeur de la clef `user:1234` en utilisant la commande `get`.

<details close>
<summary>Réponse</summary>
<b> get user:1234 </b>
</details>  
<br>  

Vous pouvez également supprimer une clef grâce à la commande `delete` suivi du nom de la clef. Par exemple, pour supprimer la clef `user1234`, vous pouvez taper la commande suivante :

```bash
del user:1234
```

NB : Si une clef n'existe pas, la commande `del` renvoie 0 (zéro).


### Cas d'usage de REDIS
On pourrait utiliser redis par exemple pour compter le nombre de visiteurs d'un site web. Pour cela il suffirait de créer une clef `visitors` par exemple et d'incrémenter sa valeur à chaque visite. Voici comment on pourrait faire :

```bash
set visitors 0
incr visitors
```

Chaque fois que quelqu'un visite le site, on peut incrémenter la valeur de la clef `visitors` en utilisant la commande `incr`. Vous pouvez tester cela en incrémentant la valeur de la clef `visitors` plusieurs fois.

La commande `decr` permet de décrémenter la valeur d'une clef. Testez la sur la clef `visitors` pour voir comment elle fonctionne.


**Remarque** Il est possible de définir la durée de vie d'une clef grâce à la commande `expire`. Par exemple, pour définir la durée de vie de la clef `visitors` à 10 secondes, vous pouvez taper la commande suivante :

```bash
expire visitors 10
```
<details close>
<summary>Réponse</summary>
<b> ttl visitors </b>
</details>  
<br>  

Vous pouvez obtenir la durée de vie d'une clef grâce à la commande `ttl`. Cette commande renvoie -1 si la durée de vie de la clef est indéfinie. Testez la commande `ttl` sur la clef `visitors` pour voir comment elle fonctionne.

Vous remarquerez qu'une clef n'est plus accessible après l'expiration de sa durée de vie. Vous pouvez vérifier cela en essayant de récupérer la valeur de la clef `visitors` après 10 secondes.




Comme dans les bases de données relationnelles, vous pouvez utiliser les commandes `Create`, `Read`, `Update` et `Delete` pour manipuler les données dans Redis.


### Quelques structures de données dans REDIS
- **La liste**   
Pour définir une liste dans redis, on utilise les commandes `lpush` et `rpush` pour ajouter des éléments à gauche et à droite de la liste respectivement. Par exemple, pour créer une liste `mesCours` contenant les éléments `BDA`, `Service WEB` et `NoSql`, vous pouvez taper les commandes suivantes :

```bash
rpush mesCours "BDA"
rpush mesCours "Service WEB"
rpush mesCours "NoSql"
```

Pour afficher les éléments de la liste `mesCours`, vous pouvez utiliser la commande `lrange` suivie du nom de la liste et des indices de début et de fin. L'indice -1 permet de récupérer tous les éléments de la liste. 

```bash
lrange mesCours 0 -1
```

Les commandes `Lpop` et `Rpop` permettent de supprimer respectivement le premier et le dernier élément de la liste. Testez ces commandes sur la liste `mesCours` pour voir comment elles fonctionnent.

Les listes peuvent contenir plusieurs occurences contrairement aux sets par exemple.

- **Le set**  
Un set est une collection d'éléments uniques. Pour créer un set dans redis, vous pouvez utiliser la commande `sadd` suivie du nom du set et des éléments à ajouter. Par exemple, pour créer un set `utilisateurs` contenant les éléments `Samir`, `Auguste` et `Marc`, vous pouvez taper les commandes suivantes :

```bash
sadd utilisateurs "Samir"
sadd utilisateurs "Auguste"
sadd utilisateurs "Marc"
```

Pour afficher les éléments du set `utilisateurs`, vous pouvez utiliser la commande `smembers` suivie du nom du set. La commande `srem` permet de supprimer un élément du set. Testez ces commandes sur le set `utilisateurs` pour voir comment elles fonctionnent. 

```bash
smembers utilisateurs
srem utilisateurs "Samir"
```

Essayez de rajouter un élément déjà existant dans le set `utilisateurs` pour voir comment redis gère les doublons.


La commande `Sunion` permet de faire l'union de plusieurs sets. Créez un nouveau set nommé `mesAmis` par exemple, remplissez le avec quelques valeurs et testez cette commande sur les sets `utilisateurs` et `mesAmis` pour voir comment elle fonctionne.

<details close>
<summary>Réponse</summary>
<b> sadd mesAmis "Aloyse" </b> <br>
<b> sadd mesAmis "Lilian" </b> <br>
<b> Sunion utilisateurs mesAmis </b> <br>
</details>
<br>

Vous pouvez retouver toutes les commandes sur la [documentation officielle de redis](https://redis.io/commands).

- **Le set ordonné**  
    Un set ordonné est une collection d'éléments uniques ordonnés par leur score. Pour créer un set ordonné dans redis, vous pouvez utiliser la commande `zadd` suivie du nom du set, du score et de l'élément à ajouter. Par exemple, pour créer un set ordonné `score` contenant les éléments `Samir`, `Auguste` et `Marc` avec les scores respectifs `1`, `2` et `3`, vous pouvez taper les commandes suivantes :

    ```bash
    zadd score 1 "Samir"
    zadd score 2 "Auguste"
    zadd score 3 "Marc"
    ```

    Quelques commandes à utiliser sur un set ordonné :
    + `zrange` : Permet de récupérer les éléments du set ordonné dans l'ordre croissant.
    + `zrevrange` : Permet de récupérer les éléments du set ordonné dans l'ordre décroissant.
    + `zrank` : Permet de récupérer le rang d'un élément dans le set ordonné.
    + `zscore` : Permet de récupérer le score d'un élément dans le set ordonné.

    Utilisation : 
    ```bash
    zrange score 0 -1
    zrevrange score 0 -1
    zrank score "Samir"
    zscore score "Samir"
    ```

- **le hash**  
    Un hash est une collection de champs associés à des valeurs. Cette structure de données est très utile dans redis pour stocker des objets contenant plusieurs attributs.  
    Pour créer un hash dans redis, vous pouvez utiliser la commande `hset` suivie du nom du hash, du champ et de la valeur à ajouter. 
    Nous allons créer un hash `user:1` contenant les champs `nom`, `prenom`,`age`et`email` avec les valeurs respectives `Dupont`, `Jean`, `25`, `JeanDupont@gmail.com`. 

    ```bash
    hset user:1 nom "Dupont"
    hset user:1 prenom "Jean"
    hset user:1 age "25"
    hset user:1 email "JeanDupont@gmail.Com"
    ```
    Quelques commandes à utiliser sur un hash :  
    - `hgetall` : Permet de récupérer tous les champs et valeurs du hash.
    - `hget` : Permet de récupérer la valeur d'un champ du hash.
    - `hvals`: Permet de récupérer toutes les valeurs du hash.
    - `hmset` : Permet de définir plusieurs champs et valeurs dans le hash.

    Utilisation : 
    ```bash
    hgetall user:1
    hget user:1 nom
    hmset user:2 nom "SECK" prenom "Aloyse" age "24" email "aloyseseck@outlook.fr"   
    hvals user:2
    ```


Attention, l'intérêt d'utiliser des bases de données NoSql est justement le fait qu'il n'y a pas de schéma prédéfini, pas de jointures, pas de contraintes d'intégrité, etc. Il est donc possible de stocker des données de différentes natures dans une même base de données. 

- **Les Pub/Sub**  
    Ces structures de données sont très utilisées dans des application <temps réel> c'est à dire des applications qui doivent réagir en temps réel à des événements comme par exemple des échanges de message.

    On va simuler l'interaction de deux clients. Pour cela, lancer le redis-cli dans deux terminaux différents. 
    + <u>Etape 1:</u>  
    Sur le terminal 1, lancer la commande suivante :
    ```bash
    subscribe mescours user:1
    ```
    Cette commande permet de s'abonner à deux canaux `mescours` et `user:1`. Le terminal1 qu'on peut appeler client1 est maintenant en attente de messages sur ces deux canaux.


    + <u>Etape 2:</u>
    Sur le terminal 2, lancer la commande suivante :
    ```bash
    publish mescours "Un nouveau cours sur MongoDB"
    ```

    Vous devriez voir le message `Un nouveau cours sur MongoDB` s'afficher sur le terminal 1. Cela signifie que le client1 a bien reçu le message publié par le client2.

    + Il est possible de spécifier le client qui doit recevoir le message en utilisant la commande `publish` suivie du nom du client et du message à envoyer comme suit :  
        ```bash
        publish user:1 "Bonjour user 1"
        ```
    + Il est également possible de s'abonner à plusieurs canaux qui commencent par un certain pattern grâce à la commande psubscribe.  
    **Cas pratique :**   
    Exécuter la commande suivante sur le client1
    ```bash
    psubscribe mes*
    ```
    Cette commande permet de s'abonner à tous les canaux qui commencent par `mes`. Maintenant essayez de lancer la commande publish sur le client2 en utilisant un canal qui commence par `mes` pour voir si le client1 reçoit le message.

    <details close>
    <summary>Réponse</summary>
    
    <b>publish mesNotes "Une nouvelle note est disponible" </b> <br>
    

## **POINTS IMPORTANTS** : </u>
- La fonction `KEYS *` permet d'afficher toutes les clefs créées durant une session
- REDIS mets à disposition 16 bases de données numérotées de 0 à 15. Vous pouvez changer de base de données en utilisant la commande `select` suivi du numéro de la base de données. 



## Quatre autres fonctionnalités dans REDIS
- **Les transactions**    

    Les transactions permettent de regrouper plusieurs commandes dans une seule transaction atomique. Pour commencer une transaction, vous pouvez utiliser la commande `multi`. Ensuite, vous pouvez ajouter des commandes à la transaction en utilisant les commandes `set`, `get`, etc. Enfin, vous pouvez exécuter la transaction en utilisant la commande `exec`. Si une des commandes de la transaction échoue, toutes les commandes précédentes sont annulées. Dans l'exemple qui suit, on va créer une clef `compteur` puis l'incrémenter dans une transaction.

    ```bash
    multi
    set compteur 0
    incr compteur
    exec
    ```

- **Les scripts Lua**  

    Redis permet d'exécuter des scripts Lua. Pour exécuter un script Lua, vous pouvez utiliser la commande `eval` suivie du script Lua et des arguments. Par exemple, pour incrémenter la valeur de la clef `compteur` en utilisant un script Lua, vous pouvez taper la commande suivante :

    ```bash
    eval "return redis.call('incr', KEYS[1])" 1 compteur
    ```
    La commande `eval` prend trois arguments : le script Lua, le nombre de clefs utilisées par le script et les arguments du script.

- **Les pipelines**

    Les pipelines permettent d'envoyer plusieurs commandes à redis en une seule fois. Pour commencer un pipeline, vous pouvez utiliser la commande `pipeline`. Ensuite, vous pouvez ajouter des commandes au pipeline en utilisant les commandes `set`, `get`, etc. Enfin, vous pouvez exécuter le pipeline en utilisant la commande `exec`. Les pipelines sont très utiles pour réduire le temps de latence lors de l'envoi de plusieurs commandes à redis. Dans l'exemple qui suit, on va incrémenter la valeur de la clef `compteur` en utilisant un pipeline.

    ```bash
    pipeline
    set compteur 0
    incr compteur
    exec
    ```

- **Les notifications**

    Les notifications permettent de recevoir des notifications lorsqu'une commande est exécutée sur une clef. Pour activer les notifications, vous pouvez utiliser la commande `config set notify-keyspace-events` suivie des événements à surveiller. Par exemple, pour surveiller les événements de type `K`, vous pouvez taper la commande suivante :

    ```bash
    config set notify-keyspace-events KE
    ```

    Les événements de type `K` correspondent aux événements de type `KeySpace`. Vous pouvez également surveiller les événements de type `E` qui correspondent aux événements de type `Keyevent`. Les notifications sont très utiles pour suivre les modifications apportées à une clef dans redis.

    





