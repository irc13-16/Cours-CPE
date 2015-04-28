#DP Etat

￼￼On évite avec de DP d'avoir des if / switch etc, on naviguant d'état en état.

Le mieux est de le faire sur papier avant de le dev pour mieux définir ses états et les actions pour passer d'un état à un autre.


￼￼Etat.java est une interface dans laquelle on déclare toutes les fonctions pour passer d'un état à un autre.
Chaque Etat implémente l'interface, et c'est donc dans cette fonction dans l'état qu'on va effectuer nos traitements et rediriger vers un autre état.
Par exemple, dans l'état EtatParti, dans la fonction payer on peut juste faire un print expliquant que le client a déjà payé son billet. Mais dans la fonction payer, de l'état EtatReservé, on pourra faire une fonction qui envoie en base de données les infos de paiement etc donc :
Paiement();
Puis setEtat(this.billet.etatConfirmé); pour renvoyer au prochain état.
Dans Billet.java on déclare en attribut tous les états possible, et un état courant.
￼￼￼￼Dans le constructeur, on peut déclarer un état courant, dans notre cas, ce sera l'état "EtatDébut".
