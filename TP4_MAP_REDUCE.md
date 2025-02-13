# MAP REDUCE AVEC COUCHDB

## Exercice 1  
Soit une matrice \( M \) de dimension \( N \times N \) représentant les liens d'un très grand nombre de pages web.  
Chaque lien est étiqueté par un poids (importance).

### 1. Modèle JSON pour représenter la matrice \( M \)  

Pour représenter la matrice \( M \) sous forme de documents structurés dans CouchDB, nous utilisons une collection \( C \) où chaque document représente une ligne de la matrice.  

Chaque document contient les informations suivantes :  

- **_id** : Un identifiant unique pour la page \( P_i \).
- **type** : Type du document (optionnel, ici "page" pour classification).
- **links** : Une liste d'objets représentant les liens sortants de la page \( P_i \).  
  Chaque objet contient :
  - **target** : L'identifiant de la page cible.
  - **weight** : Le poids attribué à ce lien.

#### Exemple de document JSON pour une page \( P_i \) :

```json
{
  "_id": "P_i",
  "type": "page",
  "links": [
    {
      "target": "P_j",
      "weight": 0.5
    },
    {
      "target": "P_k",
      "weight": 0.3
    }
    // Ajout d'autres liens si nécessaire
  ]
}
