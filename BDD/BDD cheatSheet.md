# BDD CheatSheet
Ce document ne résume pas l'intégralité du cours, seulement les points qui me semble important et/ou facilement oublié.

## Comment lire le code de ce document
Le code utilise les parenthéses avec les même régles qu'en mathématiques
- < > : champ obligatoire
- [  ] : champ facultatif
- | : OU logique
- & : ET Logique

## Merise
Merise est une méthode de modélisation. Elle permet de modéliser une base de donnée de façon robuste afin qu'elle soit durable dans le temps.

### MCD
### MLD
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
Drop If Exists Personne On Cascade;
```

- La clause _if exists_ est facultative et permet d'executer le drop uniquement si la table est déja présente
- La clausese _on cascade_ est facultative et permet de supprimer les table en contraintes externe avant celle-ci afin de ne pas supprimer de lien manquants

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
