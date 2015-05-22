# Les designs pattern **WORK IN PROGRESS**

Il existe différent types de design pattern: les modèles structurels, les modèles de créations, les modèles de comportements et puis divers inclassables.
On va voir ici les différents design pattern avec une brève description et le schéma uml d'exemple (ils sont issue de https://github.com/domnikl/DesignPatternsPHP vous pouvez voir le code de chaque exemple aussi):


## Modèles structurels

### Adapter
**Description:** L’adapter est un moyen commode de faire fonctionner un objet avec une interface qu'il ne possède pas. L'idée est de concevoir une classe supplémentaire qui se charge d'implémenter la bonne interface (l'adapteur) et d'appeler les méthodes correspondantes dans l'objet à utiliser (l'adapté).

**Exemple**:
Dans cette exemple on veut qu'un livre électronique (`EBookInterface`) puisse devenir un livre papier pour ça il nous faut un adapteur qui implémente l'interface `PaperBookInterface` et qui encapsule une implémentation de `EBookInterface` (ex: `Kindle`). Les méthodes `open` et `turnPage` dans le `EBookAdapter` appelleront respectivement les méthodes `pressStart` et `pressNext` de l'objet encapsulé.

![adapter exemple](https://rawgit.com/domnikl/DesignPatternsPHP/master/Structural/Adapter/uml/uml.png)

### Bridge (pas essentiel)
**Description**: Sert à découpler une abstraction de son implémentation. (Principe du polymorphisme basique)

**Exemple**: Deux implémentations possible, en implémentant une interface ou en héritant d'une classe abstraite (**Notez** le logo de l'image d'une classe abstraite sur le schéma)

![Bridge exemple](https://rawgit.com/domnikl/DesignPatternsPHP/master/Structural/Bridge/uml/uml.png)

### Composite
**Description**: Permet de traiter un groupe d'objets de la même façons qu'une seule instance de cet objet. (Swing en java utilise ce principe)
On peut voir ce pattern comme un arbre de même composant.

**Exemple**: On veut créer un système de formulaire qui va donc contenir des champs pour ça on va donc faire une classe abstraite `FormElement`. Il nous faudra aussi le cadre sur lequel on va mettre des éléments ici ce sera `Form` qui va pouvoir avoir d'autre `FormElement` (Ce serait la branche dans un arbre). Enfin, on va faire des champs en plus qui seront contenu dans le `Form` ici ça sera un champ text (`TextElement`) et champ input (`InputElement`), c'est deux derniers éléments pourront être vue comme des feuilles de l'arbre.

![Composite exemple](https://rawgit.com/domnikl/DesignPatternsPHP/master/Structural/Composite/uml/uml.png)

### Decorator
**Description**: Permet d'ajouter de nouvelle(s) fonctionnalité(s) à une instance de classe (j'ajouterais sur une classe où l'on a pas le contrôle comme une librairie qu'on a ajouté sur son projet).

**Exemple**: Imaginons qu'on a un webservice de type REST (`Webservice`) qui a une méthode `renderData`qui retourne des données brutes et on aimerais que ces données sortes en XML ou en Json pour ça il nous faut donc ajouter un décorateur pour ajouter ces fonctionnalités. 
On va faire deux décorateurs qui vont hérité d'une classe abstraite (`Decorator`) qui va s'occuper, par défaut, d'encapsuler notre webservice. `RenderInXml` va "surcharger" la méthode `renderData` retourner les données brutes reçus par la classe encapsulé en XML et `RenderInJson` fera pareil pour faire un rendu en json.

**Note**: `Decorator` et `Webservice` implémentent tout deux l'interface `RendererInterface` 

![Decorator exemple](https://rawgit.com/domnikl/DesignPatternsPHP/master/Structural/Decorator/uml/uml.png)

## Sources

 - http://abrillant.developpez.com/tutoriel/java/design/pattern/introduction
 - Wikipédia
 - https://github.com/domnikl/DesignPatternsPHP (pour avoir les diagrammes UML jolie et clair)
 - Mon cerveau :p (@ArthurHlt)
