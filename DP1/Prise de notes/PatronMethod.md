#DP Patron Method
###Exemple d'utilisation : 

On a une classe BoissonCafeinee (abstraite), car ça peut être thé / café par exemple, qui a une methode suivreRecett
e();

```java
	public abstract class BoissonCafeinee {
	  
		final void suivreRecette() {
			faireBouillirEau();
			preparer();
			verserDansTasse();
			ajouterSupplements();
		}
	 
		abstract void preparer();
	  
		abstract void ajouterSupplements();
	 
		void faireBouillirEau() {
			System.out.println("Portage de l'eau à ébullition");
		}
	  
		void verserDansTasse() {
			System.out.println("Remplissage de la tasse");
		}
	}
```
>Donc le truc c'est de mettre en abstrait les méthodes de la procédures qui peuvent varier. En l'occurrence : preparer et ajouterSupplements, qui seront modifier dans les classes héritées (dérivées) café et thé.

```java
	public class Cafe extends BoissonCafeinee {
	public void preparer() {
		System.out.println("Passage du café");
	}
	public void ajouterSupplements() {
		System.out.println("Ajout du lait et du sucre");
	}
}
``` 
