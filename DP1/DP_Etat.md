#DP Etat

On évite avec de DP d'avoir des if / switch etc, on naviguant d'état en état.
Le mieux est de le faire sur papier avant de le dev pour mieux définir ses états et les actions pour passer d'un état à un autre.


Voici les classes du projet : 
+ Billet.java
+ Etat.java _(interface)_
+ EtatAnnule.java 
+ EtatConfirme.java
+ EtatDebut.java
+ EtatParti.java
+ EtatReserve.java
+ TestBillet.java _(main)_

_Etat.java_ est une interface dans laquelle on déclare toutes les fonctions pour passer d'un état à un autre.
```java
    public interface Etat {
    public void reserver();
    public void payer();
    public void partir();
    public void annuler();
    public void modifier();
}
```
Chaque Etat implémente l'interface, et c'est donc dans cette fonction dans l'état qu'on va effectuer nos traitements et rediriger vers un autre état.
Par exemple, dans l'état EtatParti, dans la fonction payer on peut juste faire un print expliquant que le client a déjà payé son billet. Mais dans la fonction payer, de l'état EtatReservé, on pourra faire une fonction qui envoie en base de données les infos de paiement etc donc :
Paiement();

Puis setEtat(this.billet.etatConfirmé); pour renvoyer au prochain état.

Dans _Billet.java_ on déclare en attribut tous les états possible, et un état courant.

```java
public class Billet {
    Etat etatReserve;
    Etat etatParti;
    Etat etatConfirme;
    Etat etatAnnule;
    Etat etatDebut;
    Etat etat = etatDebut;
  
    public Billet()
    {
       etatDebut = new EtatDebut(this);
       etatAnnule = new EtatAnnule(this);
       etatReserve = new EtatReserve(this);
       etatConfirme = new EtatConfirme(this);
       etatParti = new EtatParti(this);
    }
```
Dans le constructeur, on peut déclarer un état courant, dans notre cas, ce sera l'état "EtatDébut".
