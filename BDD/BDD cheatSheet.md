# BDD CheatSheet
Ce document ne résume pas l'intégralité du cours, seulement les points qui me semble important et/ou facilement oublié.

## Comment lire le code de ce document
Le code utilise les parenthéses avec les même régles qu'en mathématiques
- < > : champ obligatoire
- [  ] : champ facultatif
- | : OU logique
- & : ET Logique

## Formes normales
Le but essentiel de la normalisation est d'éviter les anomalies transactionnelles pouvant découler d'une mauvaise modélisation des données et ainsi éviter un certain nombre de problèmes potentiels tels que les anomalies de lecture, les anomalies d'écriture, la redondance des données et la contre-performance. Il existe 5+2 FN.

####  1FN – Première forme normale
Relation dont tous les attributs :
- toutes les données sont atomiques ;
- contiennent une valeur scalaire (les valeurs ne peuvent pas être divisées en plusieurs sous-valeurs dépendant également individuellement de la clé primaire) ;
- contiennent des valeurs non répétitives (le cas contraire consiste à mettre une liste dans un seul attribut) ;
sont constants dans le temps (utiliser par exemple la date de naissance plutôt que l'âge).

_Le non-respect des deux premières conditions de la 1FN rend la recherche parmi les données plus lente parce qu'il faut analyser le contenu des attributs. La troisième condition quant à elle évite qu'on doive régulièrement mettre à jour les données._

####  2FN – Deuxième forme normale
Respecte la deuxième forme normale, la relation respectant la première forme normale et respectant le principe suivant :

Les attributs d'une relation sont divisés en deux groupes : le premier groupe est composé de la clé (un ou plusieurs attributs). Le deuxième groupe est composé des autres attributs (éventuellement vide). La deuxième forme normale stipule que tout attribut du deuxième groupe ne peut pas dépendre d'un sous-ensemble (strict) d'attribut(s) du premier groupe. En d'autres termes : « Un attribut non clé ne dépend pas d'une partie de la clé ».

_Le non-respect de la 2FN entraîne une redondance des données qui encombrent alors inutilement la mémoire et l'espace disque._

####  3FN – Troisième forme normale
Respecte la troisième forme normale, la relation respectant la seconde forme normale et respectant le principe suivant:

Les attributs d'une relation sont divisés en deux groupes : le premier groupe est composé de la clé (un ou plusieurs attributs). Le deuxième groupe est composé des autres attributs (éventuellement vide). La troisième forme normale stipule que tout attribut du deuxième groupe ne peut pas dépendre que d'un sous-ensemble (strict) d'attribut(s) du deuxième groupe. En d'autres termes : « Un attribut non clé ne dépend pas d'un ou plusieurs attributs ne participant pas à la clé ».

_Le respect de la 3FN ne garantit pas une absence de redondance des données d'où la FNBC._

## Merise
Merise est une méthode de modélisation. Elle permet de modéliser une base de donnée de façon robuste afin qu'elle soit durable dans le temps.

### MCD (Modèle Conceptuel des Données)
- Est normalisé (formes normales) afin de limiter la redondance des données
- Rappel : « ON SORT TOUT » (entités)
- Attention notamment à :
  - Respecter les 3 premièe FN 
  - Pas de propriétés multivaluées (pas de propriété ADRESSE par exemple, mais RUE, CP, VILLE, etc.)
  - Les propriétés doivent être constantes dans le temps (pas de propriété AGE mais DATE_NAISSANCE)

### MLD (Modèle Logique des Données) 
- Dépend du type de SGBD choisi (hiérarchique, relationnel, objet, etc.)
- Dans les cas des SGBD relationnels : MLDR (MLD relationnel)
- Simple transformation (mathématique) du MCD 

**/!\ pas d’optimisation à ce niveau /!\**

**/!\ On ne supprime/ajoute pas d’entité (relation) à ce niveau /!\**

### MPD (Modèle Physique des Données) 
- Transposition du MLDR propre au SGBD utilisé
    - Doit différer selon le SGBD utilisé
- C’est à ce niveau **ET SEULEMENT A CE NIVEAU** que l’on va ajouter des règles d’optimisation
 
Quelques règles simples (non exhaustives) :
- Conservation ou non des tables de référence ?
- Si le liste est susceptible d’évoluer, on conserve. Sinon, pas la peine (remplacement par une contrainte de validation CHECK).
- Limitation des clés primaires portant sur plusieurs champs à 1 champ (ou 2 champs au maximum) pour éviter d’utiliser de gros index lors des jointures.
- Optimiser ne revient généralement pas qu’à supprimer des tables mais aussi à en ajouter (sortir des attributs) !!

## SQL
La table d'exemple sera celle-ci :

**Personne**

Nom  | Prenom | adresse     | cp    | Ville | Mail
---- | ------ | ----------- | ----- | ----- | --------------------
Ciol | Loic   | Rue du Swag | 69003 | Lyon  | swaginette@gmail.com

### les types
### Les opérateurs
- **'='** : Opérateur arithmétique et comparaison exacte de chaine
- **'+' , '-' , '/', '*'** : Opérations arithémituqes
- **'LIKE'** : Opérateur pour une chaine de caractére. Le % à le role de joker (équivalent du _ dans d'autres langages). S'utilise uniquement avec des simples quote_

### DDL (Database Definition Language)
#### INSERT INTO
Permet d'inserer une ligne dans un table de la base de données séléctionnée

```SQL
insert into Personne('nom','prenom','adresse','cp','ville','mail')
values('Ben','Obab','rue du sud','89000','Toulon','Ben.Obab@supermail.com');
```

La syntaxe _Persone()_ est acceptée pour une insertion mais tous les champs devront être remplis et dans l'ordre de la table.

#### UPDATE
Permet de modifier une ou plusieures lignes d'une table

```SQL
Update Personne
set mail='swaginator@cpe.fr'
where Nom='Ciol' AND Prenom='Loic';
```

- la clause Where est facultative et concernera tous les champs si elle est omise

#### DELETE
Permet de supprimer une ou plusieures lignes de la table

```SQL
delete from Personne
Where Nom='Ciol' AND Prenom='Loic';
```

- la clause Where est facultative et concernera tous les champs si elle est omise. Cela revient à faire un _truncate_

#### DROP
Permet de supprimer une table

```SQL
Drop If Exists Personne Cascade;
```

- La clause _if exists_ est facultative et permet d'executer le drop uniquement si la table est déja présente
- La clause _cascade_ est facultative et permet de supprimer les table en contraintes externe avant celle-ci afin de ne pas supprimer de lien manquants

### Requêtes
#### Simple
La Clause _select_ permet de faire une query et de retourner des données. La structure type d'une requête SQL est la suivante :

```SQL
SELECT <(champs,champs)|*> [AS <champs,champs>]
from <DATABASE>
[WHERE <CONDITION>]
[ORDER BY (ASC|DESC)];
```

- _AS_ permet de renommer les colonnes lors de l'affichage du résultats
- _Where_ permet de filtrer la requête
- _Order by_ permet de classer par ordre croissant,alphabétique ou décroissant

#### Jointe
Les requêtes jointes permettent de croiser les données entre plusieurs tables avec un ou plusieurs champs de liaison

```SQL
Select *
from Personne p join Client c on p.nom=c.nom;
```

OU

```SQL
Select *
from Personne p join Client c
where p.nom=c.nom
```

#### Groupée
Les requêtes groupée permettent de faire des calculs et statistiques en groupant les lignes. Il existent de nombreuses fonction pour cela : _max(), min(), moy(), count(), ..._

#### Synchronisé
## Evaluation des requêtes
## Administration d'une base de données
