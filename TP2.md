# Utilisation de MongoDB

## Déploiement de MongoDB avec Docker
Nous allons utiliser Docker pour déployer une instance de MongoDB.  
Voici les étapes à suivre : 

### 1. Télécharger l'image MongoDB

```sh
docker pull mongo
```

### 2. Exécuter un conteneur MongoDB
Cette commande permet de démarrer un conteneur MongoDB en spécifiant un nom, un port de redirection et des variables d'environnement pour l'authentification.

```sh
docker run -d --name mongodb -p 27017:27017 \
  -e MONGO_INITDB_ROOT_USERNAME=admin \
  -e MONGO_INITDB_ROOT_PASSWORD=password \
  mongo
```

### 3. Copier un fichier JSON dans le conteneur
Une fois que le conteneur est démarré, vous pouvez copier un fichier JSON dans le conteneur en utilisant la commande `docker cp`.

```sh
docker cp /mnt/c/Users/mingj/films.json mongodb:/films.json
```

### 4. Importer des données JSON dans MongoDB
Maintenant, vous pouvez importer les données JSON dans MongoDB en utilisant la commande `mongoimport`.

```sh
docker exec -it mongodb mongoimport \
  --db lesfilms \
  --collection films \
  --file /films.json \
  --jsonArray \
  -u admin -p password \
  --authenticationDatabase admin
```

### 5. Accéder au shell MongoDB

```sh
docker exec -it mongodb mongosh
```

### 6. S'authentifier dans MongoDB

```sh
use admin
db.auth("admin", "password")
```

### 7. Utiliser la base de données `lesfilms`

```sh
use lesfilms
```
Cette commande permet de basculer vers la base de données `lesfilms`.

## Réponses aux questions

### Question 1 :  Vérifier que le données ont été importées, en les comptant par exemple. 

```sh
lesfilms> db.films.countDocuments()
278
```

### Question 2 : Commande findOne()

```js
lesfilms> db.films.findOne()
{
  _id: 'movie:38',
  title: 'Eternal Sunshine of the Spotless Mind',
  year: 2004,
  genre: 'Science-Fiction',
  summary: "Joël et Clémentine ne voient plus que les mauvais côtés de leur tumultueuse histoire d'amour, au point que celle-ci fait effacer de sa mémoire toute trace de cette relation. Effondré, Joël contacte l'inventeur du procédé Lacuna, le Dr. Mierzwiak, pour qu'il extirpe également de sa mémoire tout ce qui le rattachait à Clémentine. Deux techniciens, Stan et Patrick, s'installent à son domicile et se mettent à l'œuvre, en présence de la secrétaire, Mary. Les souvenirs commencent à défiler dans la tête de Joël, des plus récents aux plus anciens, et s'envolent un à un, à jamais.Mais en remontant le fil du temps, Joël redécouvre ce qu'il aimait depuis toujours en Clémentine - l'inaltérable magie d'un amour dont rien au monde ne devrait le priver. Luttant de toutes ses forces pour préserver ce trésor, il engage alors une bataille de la dernière chance contre Lacuna...",
  country: 'US',
  director: {
    _id: 'artist:201',
    last_name: 'Gondry',
    first_name: 'Michel',
    birth_date: 1963
  },
  actors: [
    { last_name: 'Ruffalo', first_name: 'Mark', birth_date: 1967 },
    { last_name: 'Winslet', first_name: 'Kate', birth_date: 1975 },
    { last_name: 'Dunst', first_name: 'Kirsten', birth_date: 1982 },
    { last_name: 'Carrey', first_name: 'Jim', birth_date: 1962 }
  ],
  grades: [
    { note: 27, grade: 'A' },
    { note: 67, grade: 'D' },
    { note: 38, grade: 'B' },
    { note: 81, grade: 'C' }
  ]
}
```

### Question 3 : Afficher la liste des films d'action
```js
lesfilms> db.films.find({ genre: "Action" }).pretty()
[
  {
    _id: 'movie:98',
    title: 'Gladiator',
    year: 2000,
    genre: 'Action',
    summary: "Le général romain Maximus est le plus fidèle soutien de l'empereur Marc Aurèle, qu'il a conduit de victoire en victoire avec une bravoure et un dévouement exemplaires. Jaloux du prestige de Maximus, et plus encore de l'amour que lui voue l'empereur, le fils de Marc Aurèle, Commode, s'arroge brutalement le pouvoir, puis ordonne l'arrestation du général et son exécution. Maximus échappe à ses assassins mais ne peut empêcher le massacre de sa famille. Capturé par un marchand d'esclaves, il devient gladiateur et prépare sa vengeance.",
    country: 'GB',
    director: {
      _id: 'artist:578',
      last_name: 'Scott',
      first_name: 'Ridley',
      birth_date: 1937
    },
    actors: [
      { last_name: 'Harris', first_name: 'Richard', birth_date: 1930 },
      { last_name: 'Crowe', first_name: 'Russell', birth_date: 1964 },
      { last_name: 'Nielsen', first_name: 'Connie', birth_date: 1965 },
      { last_name: 'Reed', first_name: 'Oliver', birth_date: 1938 },
      { last_name: 'Jacobi', first_name: 'Derek', birth_date: 1938 },
      { last_name: 'Hounsou', first_name: 'Djimon', birth_date: 1964 },
      { last_name: 'Phoenix', first_name: 'Joaquin', birth_date: 1974 }
    ],
    grades: [
      { note: 36, grade: 'C' },
      { note: 59, grade: 'B' },
      { note: 42, grade: 'F' },
      { note: 22, grade: 'C' }
    ]
  },
  {
    _id: 'movie:180',
    title: 'Minority Report',
    year: 2002,
    genre: 'Action',
    ...
  },
...
Type "it" for more
```

### Question 4 : Afficher le nombre de films d'action
```js
lesfilms>  db.films.countDocuments({ genre: "Action" })
36
```

### Question 5 : Afficher les films d'action produits en France
```js
lesfilms> db.films.find({ genre: "Action", country: "FR" }).pretty()
[
  {
    _id: 'movie:4034',
    title: "L'Homme de Rio",
    year: 1964,
    genre: 'Action',
    summary: "Le deuxième classe ...",
    country: 'FR',
    director: {
      _id: 'artist:34613',
      last_name: 'de Broca',
      first_name: 'Philippe',
      birth_date: 1933
    },
    actors: [
      {
        last_name: 'Belmondo',
        first_name: 'Jean-Paul',
        birth_date: 1933
      },
      { last_name: 'Celi', first_name: 'Adolfo', birth_date: 1922 },
      { last_name: 'Servais', first_name: 'Jean', birth_date: 1910 },
      {
        last_name: 'Dorléac',
        first_name: 'Françoise',
        birth_date: 1942
      },
      { last_name: 'Renant', first_name: 'Simone', birth_date: 1911 }
    ],
    grades: [
      { note: 4, grade: 'D' },
      { note: 30, grade: 'E' },
      { note: 34, grade: 'E' },
      { note: 28, grade: 'F' }
    ]
  },
 ...
 ```

### Question 6 : Afficher les films d'action produits en France en 1963
```js
lesfilms> db.films.find({ genre: "Action", country: "FR", year: 1963 }).pretty()
[
  {
    _id: 'movie:25253',
    title: 'Les tontons flingueurs',
    year: 1963,
    genre: 'Action',
    summary: "Sur son lit de mort, le Mexicain fait promettre à son ami d'enfance, Fernand Naudin, de veiller sur ses intérêts et sa fille Patricia. Fernand découvre alors qu'il se trouve à la tête d'affaires louches dont les anciens dirigeants entendent bien s'emparer. Mais, flanqué d'un curieux notaire et d'un garde du corps, Fernand impose d'emblée sa loi. Cependant, le belle Patricia lui réserve quelques surprises !",
    country: 'FR',
    director: {
      _id: 'artist:18563',
      last_name: 'Lautner',
      first_name: 'Georges',
      birth_date: 1926
    },
    actors: [
      { last_name: 'Ventura', first_name: 'Lino', birth_date: 1919 },
      { last_name: 'Frank', first_name: 'Horst', birth_date: 1929 },
      { last_name: 'Dalban', first_name: 'Robert', birth_date: 1903 },
      { last_name: 'Blier', first_name: 'Bernard', birth_date: 1916 },
      { last_name: 'Rich', first_name: 'Claude', birth_date: 1929 },
      { last_name: 'Sinjen', first_name: 'Sabine', birth_date: 1942 },
      { last_name: 'Lefebvre', first_name: 'Jean', birth_date: 1920 },
      { last_name: 'Bertin', first_name: 'Pierre', birth_date: 1891 },
      { last_name: 'Blanche', first_name: 'Francis', birth_date: 1919 }
    ],
    grades: [
      { note: 35, grade: 'C' },
      { note: 48, grade: 'F' },
      { note: 64, grade: 'C' },
      { note: 42, grade: 'F' }
    ]
  }
]
```	

### Question 7 : Afficher les films d'action sans les grades
```js
lesfilms> db.films.find(
...   { genre: "Action", country: "FR" },  // Filtre
...   { grades: 0 }                        // Projection: on exclut les grades
... ).pretty()
[
  {
    _id: 'movie:4034',
    title: "L'Homme de Rio",
    year: 1964,
    genre: 'Action',
    summary: "Le deuxième classe Adrien Dufourquet est témoin de l'enlèvement de sa fiancée Agnès, fille d'un célèbre ethnologue. Il part à sa recherche, qui le mène au Brésil, et met au jour un trafic de statuettes indiennes.",
    country: 'FR',
    director: {
      _id: 'artist:34613',
      last_name: 'de Broca',
      first_name: 'Philippe',
      birth_date: 1933
    },
    actors: [
      {
        last_name: 'Belmondo',
        first_name: 'Jean-Paul',
        birth_date: 1933
      },
      { last_name: 'Celi', first_name: 'Adolfo', birth_date: 1922 },
      { last_name: 'Servais', first_name: 'Jean', birth_date: 1910 },
      {
        last_name: 'Dorléac',
        first_name: 'Françoise',
        birth_date: 1942
      },
      { last_name: 'Renant', first_name: 'Simone', birth_date: 1911 }
    ]
  },
  ...
]
```

### Question 8 : Afficher les films d'action réalisés en France sans les identifiants et les grades
```js
lesfilms> db.films.find(
...   { genre: "Action", country: "FR" },  // Filtre
...   { _id: 0, grades: 0 }                // Projection: exclut `_id` et `grades`
... ).pretty()
[
  {
    title: "L'Homme de Rio",
    year: 1964,
    genre: 'Action',
    summary: "Le deuxième classe Adrien Dufourquet est témoin de l'enlèvement de sa fiancée Agnès, fille d'un célèbre ethnologue. Il part à sa recherche, qui le mène au Brésil, et met au jour un trafic de statuettes indiennes.",
    country: 'FR',
    director: {
      _id: 'artist:34613',
      last_name: 'de Broca',
      first_name: 'Philippe',
      birth_date: 1933
    },
    actors: [
      {
        last_name: 'Belmondo',
        first_name: 'Jean-Paul',
        birth_date: 1933
      },
      { last_name: 'Celi', first_name: 'Adolfo', birth_date: 1922 },
      { last_name: 'Servais', first_name: 'Jean', birth_date: 1910 },
      {
        last_name: 'Dorléac',
        first_name: 'Françoise',
        birth_date: 1942
      },
      { last_name: 'Renant', first_name: 'Simone', birth_date: 1911 }
    ]
  },
...]
```

### Question 9 : Afficher les titres et les grades des films d'action réalisés en France sans leurs identifiants
```js
lesfilms> db.films.find(
...   { genre: "Action", country: "FR" },  // Filtre
...   { _id: 0, title: 1, grades: 1 }      // Projection: inclut `title` et `grades`, exclut `_id`
... ).pretty()
[
  {
    title: "L'Homme de Rio",
    grades: [
      { note: 4, grade: 'D' },
      { note: 30, grade: 'E' },
      { note: 34, grade: 'E' },
      { note: 28, grade: 'F' }
    ]
  },
  {
    title: 'Nikita',
    grades: [
      { note: 88, grade: 'F' },
      { note: 36, grade: 'C' },
      { note: 74, grade: 'D' },
      { note: 62, grade: 'D' }
    ]
  },
  {
    title: 'Les tontons flingueurs',
    grades: [
      { note: 35, grade: 'C' },
      { note: 48, grade: 'F' },
      { note: 64, grade: 'C' },
      { note: 42, grade: 'F' }
    ]
  }
]
```
### Question 10 :  Afficher les titres et les notes des films d’action réalisés en France et ayant obtenus une note supérieur à 10.
```js
lesfilms> db.films.find(
...   {
...     genre: "Action",
...     country: "FR",
...     "grades.note": { $gt: 10 }  // Vérifie si au moins une note est > 10
...   },
...   { _id: 0, title: 1, grades: 1 }  // Projection: Affiche uniquement `title` et `grades`
... ).pretty()
[
  {
    title: "L'Homme de Rio",
    grades: [
      { note: 4, grade: 'D' },
      { note: 30, grade: 'E' },
      { note: 34, grade: 'E' },
      { note: 28, grade: 'F' }
    ]
  },
  {
    title: 'Nikita',
    grades: [
      { note: 88, grade: 'F' },
      { note: 36, grade: 'C' },
      { note: 74, grade: 'D' },
      { note: 62, grade: 'D' }
    ]
  },
  {
    title: 'Les tontons flingueurs',
    grades: [
      { note: 35, grade: 'C' },
      { note: 48, grade: 'F' },
      { note: 64, grade: 'C' },
      { note: 42, grade: 'F' }
    ]
  }
]
``` 
**NB :** Certains films ont des notes inférieures à 10, mais ils sont inclus car au moins une note est supérieure à 10.

### Question 11 : Films produits en France ayant obtenu que des notes supérieures à 10
```js
lesfilms> db.films.find(
...   {
...     genre: "Action",
...     country: "FR",
...     "grades.note": { $not: { $lte: 10 } }  // Exclut tout film ayant une note ≤ 10
...   },
...   { _id: 0, title: 1, grades: 1 }  // Projection: affiche uniquement `title` et `grades`
... ).pretty()
[
  {
    title: 'Nikita',
    grades: [
      { note: 88, grade: 'F' },
      { note: 36, grade: 'C' },
      { note: 74, grade: 'D' },
      { note: 62, grade: 'D' }
    ]
  },
  {
    title: 'Les tontons flingueurs',
    grades: [
      { note: 35, grade: 'C' },
      { note: 48, grade: 'F' },
      { note: 64, grade: 'C' },
      { note: 42, grade: 'F' }
    ]
  }
]
```

### Question 12 : Afficher les différents genres de la base de données films
```js
lesfilms> db.films.distinct("genre")
[
  'Action',          'Adventure',
  'Aventure',        'Comedy',
  'Comédie',         'Crime',
  'Drama',           'Drame',
  'Fantastique',     'Fantasy',
  'Guerre',          'Histoire',
  'Horreur',         'Musique',
  'Mystery',         'Mystère',
  'Romance',         'Science Fiction',
  'Science-Fiction', 'Thriller',
  'War',             'Western'
]
```
Ci-dessus, l'opérateur `$distinct` est utilisé pour renvoyer une liste distincte des valeurs pour un champ spécifié dans une collection. Dans ce cas, nous avons utilisé l'opérateur `$distinct` pour renvoyer une liste distincte des genres de films dans la collection `films`.

### Question 13 : Afficher les différents grades attribués
```js
lesfilms> db.films.distinct("grades.grade")
[ 'A', 'B', 'C', 'D', 'E', 'F' ]
```

### Question 15 : Afficher tous les films qui n'ont pas de résumé
```js
lesfilms> db.films.find(
.pretty()
...   { $or: [ { summary: { $exists: false } }, { summary: "" } ] }
... ).pretty()
[
  {
    _id: 'movie:181812',
    title: 'Star Wars, épisode IX',
    year: 2019,
    genre: 'Science-Fiction',
    summary: '',
    country: 'GB',
    director: {
      _id: 'artist:15344',
      last_name: 'Abrams',
      first_name: 'J.J.',
      birth_date: 1966
    },
    actors: [
      { last_name: 'Hamill', first_name: 'Mark', birth_date: 1951 },
      { last_name: 'Fisher', first_name: 'Carrie', birth_date: 1956 },
      {
        last_name: 'Dee Williams',
        first_name: 'Billy',
        birth_date: 1937
      },
      { last_name: 'Isaac', first_name: 'Oscar', birth_date: 1979 },
      { last_name: 'Boyega', first_name: 'John', birth_date: 1992 },
      { last_name: 'Driver', first_name: 'Adam', birth_date: 1983 },
      { last_name: 'Ridley', first_name: 'Daisy', birth_date: 1992 },
      {
        last_name: 'Marie Tran',
        first_name: 'Kelly',
        birth_date: 1989
      }
    ],
    grades: [
      { note: 94, grade: 'B' },
      { note: 28, grade: 'A' },
      { note: 81, grade: 'B' },
      { note: 100, grade: 'B' }
    ]
  }
]
```
L'opérateur existe `$exists` est utilisé pour vérifier si un champ existe dans un document. Dans ce cas, nous avons utilisé l'opérateur `$or` pour combiner deux conditions : soit le champ `summary` n'existe pas (``$exists: false``), soit il est vide.

### Question 16 : Afficher tous les films tournés avec Leonardo DiCaprio en 1997
```js
lesfilms> db.films.find(
...   {
...     "actors.first_name": "Leonardo",
...     "actors.last_name": "DiCaprio",
...     year: 1997
...   }
... ).pretty()
[
  {
    _id: 'movie:597',
    title: 'Titanic',
    year: 1997,
    genre: 'Drame',
    summary: 'Southampton, 10 avril 1912. Le paquebot le plus grand et le plus moderne du monde, réputé pour son insubmersibilité, le « Titanic », appareille pour son premier voyage. 4 jours plus tard, il heurte un iceberg. À son bord, un artiste pauvre et une grande bourgeoise tombent amoureux.',
    country: 'US',
    director: {
      _id: 'artist:2710',
      last_name: 'Cameron',
      first_name: 'James',
      birth_date: 1954
    },
    actors: [
      { last_name: 'Winslet', first_name: 'Kate', birth_date: 1975 },
      { last_name: 'Zane', first_name: 'Billy', birth_date: 1966 },
      { last_name: 'Fisher', first_name: 'Frances', birth_date: 1952 },
      {
        last_name: 'DiCaprio',
        first_name: 'Leonardo',
        birth_date: 1974
      },
      { last_name: 'Bates', first_name: 'Kathy', birth_date: 1948 },
      { last_name: 'Stuart', first_name: 'Gloria', birth_date: 1910 }
    ],
    grades: [
      { note: 40, grade: 'C' },
      { note: 91, grade: 'B' },
      { note: 45, grade: 'D' },
      { note: 7, grade: 'A' }
    ]
  }
]
```

### Question 17 :  Afficher les films tournés avec Leonardo DiCaprio en 1997
```js
lesfilms> db.films.find( { $or: [ { "actors.first_name": "Leonardo", "actors.last_name": "DiCaprio" }, { year: 1997 }] } ).pretty()
[
  {
    _id: 'movie:184',
    title: 'Jackie Brown',
    year: 1997,
    genre: 'Crime',
    summary: "Hôtesse de l'air, Jackie Brown arrondit ses fins de mois en convoyant de l'argent liquide pour le compte de Ordell Robbie, trafiquant d'armes. Quand la police et le FBI lui signifient qu'ils comptent sur elle pour faire tomber Robbie, Jackie Brown échafaude un plan audacieux.",
    country: 'US',
    director: {
      _id: 'artist:138',
      last_name: 'Tarantino',
      first_name: 'Quentin',
      birth_date: 1963
    },
    actors: [
      { last_name: 'Tucker', first_name: 'Chris', birth_date: 1971 },
      { last_name: 'De Niro', first_name: 'Robert', birth_date: 1943 },
      { last_name: 'Grier', first_name: 'Pam', birth_date: 1949 },
      {
        last_name: 'L. Jackson',
        first_name: 'Samuel',
        birth_date: 1948
      },
      { last_name: 'Keaton', first_name: 'Michael', birth_date: 1951 },
      { last_name: 'Fonda', first_name: 'Bridget', birth_date: 1964 },
      { last_name: 'Bowen', first_name: 'Michael', birth_date: 1953 },
      { last_name: 'Forster', first_name: 'Robert', birth_date: 1941 }
    ],
    grades: [
      { note: 16, grade: 'E' },
      { note: 28, grade: 'C' },
      { note: 18, grade: 'D' },
      { note: 39, grade: 'B' }
    ]
  },
...
]
```
Pour répondre à cette question, nous avons utilisé l'opérateur `$or` pour combiner les deux conditions. L'opérateur `$or` permet de sélectionner les documents qui satisfont au moins une des conditions spécifiées.