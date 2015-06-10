# Les designs pattern **WORK IN PROGRESS**
Il existe différent types de design pattern: les modèles structurels, les modèles de créations, les modèles de comportements et puis divers inclassables. On va voir ici les différents design pattern avec une brève description et le schéma uml d'exemple (ils sont issue de [https://github.com/domnikl/DesignPatternsPHP](https://github.com/domnikl/DesignPatternsPHP) vous pouvez voir le code de chaque exemple aussi):

## Introduction
### Les 4 pilliers de la POO
- L'abstraction
- l'encapsulation
- l'héritage
- le polymorphisme

Un programme bien concu est un programme souple, extensible et facile à maintenir :

**Programmez des interfaces, non des implémentations**

Si vous programmez avec des bonnes interfaces votre code sera beaucoup plus robuste aux changements et vous pourrez utiliser certaines fonctionnalités communes efficacement. L'exemple le plus parlant est sans doute l'itérateur Java pour parcourir une collection. Pas besoin de connaitre la structure interne d'une collection parmi les nombreuses existantes pour naviguer sur les éléments qui la constitue, il suffit d'utiliser l'interface Iterator. Vous pouvez donc parcourir une liste chaînée, un tableau dynamique, une structure en arbre ou je ne sais quoi de la même manière. Avouez que c'est vraiment utile.

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
- Un composant, classe ou module, est fortement cohésif s'il est conçu autour d'un ensemble de fonctions apparentées (Fonction liés à un animal)
- il est faiblement cohésif si les fonctions n'ont pas de lien entre elles.

### Erreurs à ne pas faire
- Ne pas utiliser de _switch_ pour determiner le type de l'objet, utiliser plutôt le polymorphisme

_Exemple à ne pas faire :_

```JAVA
Animal a = getAnimal()
switch(a.getClass()){
    case "Chien":
    break;
    case "Chat":
    break;
    default:
    break;
}
```

- Code en double : Il faut sortir les méthodes en double et les mettres dans une classe mére (héritage)
- Objet "Dieu" : objet ayant toutes les resposabilités -> redistribuer les taches dans d'autres objets
- Methode trop volumineuse

## Les design pattern
Les design patterns sont des solution génériques aux problémes de conception. Ils sont à adapté selon les situations.

Il existe trois grandes familles de Design Pattern

### Design Pattern de création
Ils définissent comment instancier les objets et fournissent un moyen de découpler les objets en les rendant indépendant de la façon dont ils sont crées et en cachant leurs creations

### Design Pattern de structure
Ils définissent comment organiser les classes dans un projet. Ils sont complementaires.

### Design Pattern de comportement
Ils définissent comment organiser les objets pour que ceux-ci collaborent (distribution des responsabilités) et expliquent le fonctionnement des algorithmes impliqués.

## Modèles structurels
### Adapter
**Description:** L'adapter est un moyen commode de faire fonctionner un objet avec une interface qu'il ne possède pas. L'idée est de concevoir une classe supplémentaire qui se charge d'implémenter la bonne interface (l'adapteur) et d'appeler les méthodes correspondantes dans l'objet à utiliser (l'adapté).

**Exemple**: Dans cette exemple on veut qu'un livre électronique (`EBookInterface`) puisse devenir un livre papier pour ça il nous faut un adapteur qui implémente l'interface `PaperBookInterface` et qui encapsule une implémentation de `EBookInterface` (ex: `Kindle`). Les méthodes `open` et `turnPage` dans le `EBookAdapter` appelleront respectivement les méthodes `pressStart` et `pressNext` de l'objet encapsulé.

![adapter exemple](https://rawgit.com/domnikl/DesignPatternsPHP/master/Structural/Adapter/uml/uml.png)

### Bridge (pas essentiel)
**Description**: Sert à découpler une abstraction de son implémentation. (Principe du polymorphisme basique)

**Exemple**: Deux implémentations possible, en implémentant une interface ou en héritant d'une classe abstraite (**Notez** le logo de l'image d'une classe abstraite sur le schéma)

![Bridge exemple](https://rawgit.com/domnikl/DesignPatternsPHP/master/Structural/Bridge/uml/uml.png)

### Composite
**Description**: Permet de traiter un groupe d'objets de la même façons qu'une seule instance de cet objet. (Swing en java utilise ce principe) On peut voir ce pattern comme un arbre de même composant.

**Exemple**: On veut créer un système de formulaire qui va donc contenir des champs pour ça on va donc faire une classe abstraite `FormElement`. Il nous faudra aussi le cadre sur lequel on va mettre des éléments ici ce sera `Form` qui va pouvoir avoir d'autre `FormElement` (Ce serait la branche dans un arbre). Enfin, on va faire des champs en plus qui seront contenu dans le `Form` ici ça sera un champ text (`TextElement`) et champ input (`InputElement`), c'est deux derniers éléments pourront être vue comme des feuilles de l'arbre.

![Composite exemple](https://rawgit.com/domnikl/DesignPatternsPHP/master/Structural/Composite/uml/uml.png)

### Decorator
**Description**: Permet d'ajouter de nouvelle(s) fonctionnalité(s) à une instance de classe (j'ajouterais sur une classe où l'on a pas le contrôle comme une librairie qu'on a ajouté sur son projet).

**Exemple**: Imaginons qu'on a un webservice de type REST (`Webservice`) qui a une méthode `renderData`qui retourne des données brutes et on aimerais que ces données sortes en XML ou en Json pour ça il nous faut donc ajouter un décorateur pour ajouter ces fonctionnalités. On va faire deux décorateurs qui vont hérité d'une classe abstraite (`Decorator`) qui va s'occuper, par défaut, d'encapsuler notre webservice. `RenderInXml` va "surcharger" la méthode `renderData` retourner les données brutes reçus par la classe encapsulé en XML et `RenderInJson` fera pareil pour faire un rendu en json.

**Note**: `Decorator` et `Webservice` implémentent tout deux l'interface `RendererInterface`

![Decorator exemple](https://rawgit.com/domnikl/DesignPatternsPHP/master/Structural/Decorator/uml/uml.png)

## Sources
- [http://abrillant.developpez.com/tutoriel/java/design/pattern/introduction](http://abrillant.developpez.com/tutoriel/java/design/pattern/introduction)
- Wikipédia
- [https://github.com/domnikl/DesignPatternsPHP](https://github.com/domnikl/DesignPatternsPHP) (pour avoir les diagrammes UML jolie et clair)
- Mon cerveau :p ([@ArthurHlt](https://github.com/ArthurHlt))
