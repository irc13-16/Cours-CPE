# Design Pattern CheatSheet
## Introduction
### Les 4 pilliers de la POO
- L'abstraction
- l'encapsulation
- l'héritage
- l'encapsulation

Un programme bien concu est un programme souple, extensible et facile à maintenir :

**Concept de forte cohésion - faible couplage**

### Couplage et cohésion
#### Couplage
- Le couplage exprime la relation étroite qu'un élément (Classe,système ou sous système) entretien avec un ou des autres éléments.
- Un élément faiblement couplé ne possède pas ou peu de dépendances vis a vis d'autres éléments.

Exemple de couplage fort :

```JAVA
Animal a;
if(type==1) a= new chien();
else if (type==2) a = new Chat();
```

Exemple de couplage faible :

```JAVA
Animal a;
a = fabrique.getAnimal(type);
//L’objet fabrique gère les new.
```

#### Cohésion
- Un composant, classe ou module, est fortement cohésif s'il est conçu autour d'un ensemble de fonctions apparentées
- il est faiblement cohésif si les fonctions n'ont pas de lien entre elles.
