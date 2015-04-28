#DP Stratégie

Pour un objet, on a certaines parties qui varient, d'autres non. Par exemple pour le canard (classe abstraite), chacun de ses fils (colvert / canardPlastique) etc, a des comportements propres. 

On implémente donc ces comportements dans d'autres classes, donc le comportement doit être régi par une interface : 

```java
	public interface ComportementVol {
	public void voler();
}
```

Et on peut en définir plusieurs : 
```java
	VolerAvecDesAiles.java
	VolerReacteur.java
```

Ensuite on peut seter directement un comportement si on greffe une gorge sur notre canard en plastique par exemple : 
```java
        public void setComportementVol(ComportementVol a)
        {
            this.comportementVol = a;
            //ici on doit créer une classe locale si on veut ajouter un nouveau comportement
            class a{
                
            }
        }
        
        public void setComportementCancan(ComportementCancan a)
        {
            this.comportementCancan = a;
            
        }
``` 

La fonction dans la classe Canard (abstraite) ci dessus.

###Et son utilisation : 
```java
	canardPlastique.setComportementVol(new NePasVoler());
    canardPlastique.setComportementCancan(new Cancan());
```

Pareil pour Comportement Cancan évidemment.

Ensuite si on veut carrément créer notre propre comportement, on peut créer une classe locale (anonyme) dans notre setComportemetVol(ComportementVol a); de la classe Canard.

Pour résumer, le DP stratégie permet d'éviter de dupliquer du code dans plusieurs classe, en définissant des comportements communs à plusieurs objets.
