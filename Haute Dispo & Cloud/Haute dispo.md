# Haute disponibilité
La sureté de fonctionnement c'est prévoir les événements dangeureux et evaluer/réduire les occurences et les dommages de ces événements.


La fiabilité est la probabilité pour qu'une entité puisse accomplir une fonction requise, pour des conditions données, pendant un interval de temps donné.

On parle de défailance lorsqu'un système ne respecte plus les fonctions attendues.

Une faute est suceptible d'entrainer une défaillance et d'introduire une erreur dans le système.
- **Fiabilité**: Mesure du Fonctionnement correct du systéme
- **Sûreté**: Quoi qu'il arrive, rien de grave ne se passera
- **Disponibilité**: Mesure du temps de fonctionnement

## Critére de qualité d'un systéme
- Performances
  - Temps de réponses
  - Utilisation des ressources
  - Evolutivité

- Sureté de fonctionnement
  - Disponibilité
  - Fiabilité
  - Sûreté
  - Maintenanbilité
  - Sécurité

  _Exemple :_ 99% (web)/ 99,9% (e-commerce) / 99,99% (service mail) / 99,999% réseaux tel / 99,9999%

### Comment améliorer la disponibilité d'un systéme ?
- 99,9% -> Bonne pratique
- 99,99% -> Masquer les fautes matériels élémentaires
- 99,999% -> Maintenance Préventive et masquer les fautes matérielles et logicielles
- 99,9999% -> Masquer les défaillances de site (incendie, oxygéne) & Masquer les défaillances opérationnels globales

Temps Moyen avant défaillance MTTF (Mean Time To Failure): MTTF = Moy(Ai) Ai -> Intervalle de fonctionnement

Temps Moyen pour réparation MTTR = Moy(Bi) BI -> temps de réparation

Temps Moyen entre défaillance MTBF = Moy(Ai+Bi) BI -> temps de réparation
- Disponibilité = MTTF/(MTTF+MTTR)
- Indisponibilité = 1 - Disponibilité
- Eviter les SPOF (Single Point of Failure)
- Faire de la redondances et mettre des modules d'attente aux fautes

## Elements de calcul
- Fiabilité = R
- Défaillance = 1- R
- Module en série :  R1xR2xR3
- Module en parallele : 1 - [(1-R1)x(1-R2)x(1-R3)]

# Auteur
- ([@Aveys](https://github.com/Aveys))
- et bientôt vous ;)
