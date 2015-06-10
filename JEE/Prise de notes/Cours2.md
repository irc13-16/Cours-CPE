# Cours 2
## Synthése
- J2E permet de faire du web, de rendre du contenu dynamique
- J2E permet de faire des architectures distribuée (communication machine<->machine)
- Le composant central du web de J2E sont les servlets -> elle permetten d'intercepter les requetes HTTP et déclencher les méthodes dans ces objets
- JSP => recette de cuisine d'affichage avec des zones ou on est capable de remplacerle contenu. Une JSP est transformé en servlet par la suite
- Java beans pour partager des donnée entre servlet et JSP
- J2E adopte l'architecture MVC : model = beans, controller = servlet, view = JSP
- Complexité forte des JSP -> Apparition des JSF
- JSF -> on oublie les servlets et on distinguera par des annotations deux types de Java beans (controller et model). JSF(.xhtml) s'occupe de la vue
- On peut définir des scopes pour nos beans (Application, session, request, page) pour spécifier sa durée de vie
