# Cours 1
- ne pas utiliser de pattern quand on en as pas besoin
- MVC est pattern d'architecture et pas de conception
- qu'est ce qui qualifie une bonne conception orientée objet
  - facile à etendre
  - facile à utiliser, maintenir

- les 4 pilliers de l'approche objets
  - heritage
  - polymorphisme
  - abstraction
  - ?

## DP Patron de methode
 definit le squellette d'un algorithme et distribue des taches à ses sous-classes

### Questions
- les methodes abstraite sont dans les sous-classes
- avec le mot clé final
- Le patron qui invoque les methodes de ses classes dérivées

## DP Strategy
### Questions
1. t
2. t
3. t
4. t
5. t

## DP State
machine à état

## DP Observer
- ce aui varie est sortie en observer avec une méthode d'update
- Ne pas utiliser quand on a de fort besoins en héritage

### Code
_Observable_

```JAVA
import java.util.Observable;

/**
 * Created by arthurveys on 28/04/15.
 */
public class MeteoData extends Observable {
    public void setTemp(double temp) {
        this.temp = temp;
        setChanged();
        notifyObservers(this.temp);
    }

    private double temp=0.0;
    private double hygro;

}
```

_Observer_

```JAVA
public class MeteoUpdate implements Observer {
    @Override
    public void update(Observable o, Object arg) {
        System.out.println("Mise à jour ! \n Il fait "+ arg);
    }
}
```

On peut faire transiter un tableau d'arguments si on veut en faire passer plusieurs.

### Questions
1. observer des données
2. La question 2
  1. ce qui varie est sortie en observer avec une méthode d'update
  2. Le sujet concret connait une liste abstaite d'observer
  3. faiblement couplé car on connait le sujet concret ne connais pas les observateurs concrets
  4. Impossible de mettre d'héritage ici
  5. C'est le sujet qui informe les observateurs de mise à jour
  6. il remplace trés bien le DP State
