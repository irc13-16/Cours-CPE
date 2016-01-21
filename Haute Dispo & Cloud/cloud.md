# Cloud Computing
C'est plus une évolution qu'une révolution.

C'est l'équivalent de l'électicité ou de l'eau : L'infrastructure est mutualisée et je ne paye que ce que j'utilise vraiment.

_Les différentes offres dans le cloud : _
- SAAS : Software as a service, ex : google Docs. On vent l'utilisation du soft en fonction de l'usage (paiement à l'utilisateur, ...). Salesforce st un exemple de CRM en SAAS
- PAAS : App engine. Plateforme applicative. Paiement en fct du nombre d'users.
- IAAS : GDrive.

Avantage                      | Inconvénient
:---------------------------: | :------------------------:
Moins de déploiement          | Sécurité des données
Mutualisation des couts       | Rigidité des applications
Fiabilité                     | Cycle de vie commun à tous
Accessibles par tout le monde |
Trés forte évolution          |

Classique              | Cloud
:--------------------: | :------------------------:
Paiement d'une licence | Sécurité des données
Achats des serveurs    | Rigidité des applications
Paramétrage            | Sécurisation de l'accés
Dimensionnement        | Cycle de vie commun à tous

- se poser la question du cloud public / privé

## Architecture types et contraintes
### Contraintes pour un PAAS
- Ne pas écrire dans un fichier
- Impossible de faire des threads -> evolutivité horizontale (+ de machines)
- Temps d'éxécution limité (généralement 30sec) -> opération désynchronisées
- Eviter les bases de données relationnelles -> base de données NoSQL + MapReduce

### Opérations désynchronisées
Utilisation de files de messages pour communiquer entre les serveurs. La taille de cette file peut evoluer selon les hebergeurs.

Un manager surveille les opérations et lance ou stope les services selon la file. Il surveille également un éventuel blocage.

### BDD
Dans le cloud, la base de donnée est partagée entre les serveurs. Il y a un point d'écriture (Master) et plusieurs points de lectures (Slave). On appelle cela un fonctionnnement en Cluster.
- Une base relationnelles respecte la propriété **ACID** :
  - **Atomicité** : transaction validée ou refusée
  - **Consistance** : la base ne peut pas étre dans un état incohérentes
  - **Isolation** : Une transaction ne peut pas dépendre d'une autre transaction
  - **Durabilité** : Aprés le succés d'une transaction, le resultat ne disparait pas

Une BDD NoSQL ne respecte pas ce principe, n'a pas de schéma et donc pas de langage de requêtage évolué comme le SQL. Elles sont souvent clusterisée (lecture et écriture sur tous les noeuds) et utilise un master pour tout orchestrer.

#### MapReduce
L'algorithme MapReduce s'effectue en deux temps :
- Map : traiter les données (récupérer les données)
- Reduce : regroupe les données et les traiter

#Amazon Web services **IMPORTANT** : Je considére que cette partie n'est pas interessante et ne sera pas dans le DS. A voir si quelqu'un à le courage de le faire.
