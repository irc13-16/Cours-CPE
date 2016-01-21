# Google App Engine
C'est une plateforme de conception et d'hébergement web basé sur l'infrastructure de Google et lancé en 2008 avec un vrai support depuis 2011.

**Propriétés**
- Utilisation du web dynamique (python,go,J2EE)
- Stockage permanent sur BigTable
- Equilibrage des charges et Evolutivité auto
- Execution des appli dans des sandbox
- Files d'attente
- Planification de tâches

Son site est disponible généralement à l'adresse app-id.appspot.com

**Les services**
- Datastore
  - Persistance des données

- Transaction
  - gestion de la concurrence des modifications dans le DataStore

- Memory Cache
  - Persistance temporaire

- URL Fetch Service
  - Redirection de requete http

- Mail Service
  - Envoi et réception d'email

- Channel
  - Web socket pour Google App Engine

- Support XMPP
  - Messagerie instantanée (Google talk)

- Image Processing service
  - Service de transformation d'images

## La sandbox
Isole l'environnement de chaque APP du systéme et des autres app

_Conséquences_
- Pas de démarrage de Thread
- Pas de connexions réseaux arbitraires
- Lecture uniqument de son propre code et ressources
- Aucune connaissance de l'etat des autres apps

## Datastore
Vocabulaire:
- Entity -> Une ligne de table
- Property -> un champ d'une colone sur une ligne
- une entity peut avoir plusieurs properties
- 2 entity de même type peuvent avoir des properties différentes
- 1 entity posséde une clé unique non modifiable
- Tout est indéxé

Les requêtes sont optimisées pour travailler sur un grand volume de données mais il est impossible de faire des jointures et il faut que la property existe pour pouvoir faire un tri. De plus, on ne peut utiliser les opérateurs de recherche que sur une seul champ à la fois.

```Java
// Exemple
DatastoreService datastore = DatastoreServiceFactory.getDatastoreService();
Entity employee = new Entity("Employee");
employee.setProperty("firstName", "Antonio");
employee.setProperty("lastName", "Salieri");
Date hireDate = new Date();
employee.setProperty("hireDate", hireDate);
employee.setProperty("attendedHrTraining", true);
datastore.put(employee);
Key employeeKey = employee.getKey();
Entity employee = datastore.get(employeeKey);
// Utilisation Query afin de rassembler les éléments a appeler/filter
Query q = new Query("Person");
Filter lastNameFilter = new FilterPredicate("lastName",
FilterOperator.EQUAL, lastNameParam);
Filter heightMaxFilter = new FilterPredicate("height",
FilterOperator.LESS_THAN_OR_EQUAL, maxHeightParam);
Filter heightRangeFilter = CompositeFilterOperator.and(lastNameFilter,
heightMaxFilter);
q.setFilter(heightRangeFilter);
// Récupération du résultat de la requète à l’aide de PreparedQuery
PreparedQuery pq = datastore.prepare(q);
for (Entity result : pq.asIterable()) {
 String firstName = (String) result.getProperty("firstName");
 String lastName = (String) result.getProperty("lastName");
 Long height = (Long) result.getProperty("height");
}
```

## AppCache
C'est un stockage temporaire de clé/valeur trés rapide car stocké directement dans la RAM. De plus, c'est un systéme distribué entre toutes les instances de notre application. On peut attribuer un temps de vie à chaque couple de clé/valeur. `Attention, pas de persistance des données lors de l'arrêt du serveur`

**Bonne pratique**
- Verifier si une valeur est disponible dans le cache
- si non présent, prendre la valeur dans le datastore et la mettre dans le cache.
- si présent, utiliser la valeur du cache

```Java
 Cache cache=null;
 Map props = new HashMap();
 props.put(GCacheFactory.EXPIRATION_DELTA, 3600);
 props.put(MemcacheService.SetPolicy.ADD_ONLY_IF_NOT_PRESENT, true);
 try {
 // Récupération du Cache
 CacheFactory cacheFactory = CacheManager.getInstance().getCacheFactory();
 // création/récupération du cache suivant des propriétés spécifiques
 cache = cacheFactory.createCache(props);

 // Si aucune propriété n'est spécifiée,
 //créer/récupérer un cache comme ci-dessous
 //cache = cacheFactory.createCache(Collections.emptyMap());
 } catch (CacheException e) {
 // Traitement en cas d'erreur sur la récupération/configuration du cache
 }
 String key="objKey1"; // Définir la clé de la valeur à stocker
 String value="myValue1" // Définir l'objet à stocker
 // Mise en cache de l'objet à l'aide d'une clé
 cache.put(key, value);
 // Récupération de l'objet stocké
 value = (String)cache.get(key);
```

## Task Queue - Cron
 Permet d'effectuer des tâches en dehors des réponses aux requêtes web

**Propriétés**
- Planification possible (CRON like)
- Tentative de réexecution automatiques en cas d'echec (datastore)
- Les tâches planifiées ne support pas de multiple tentatives
- Une tâche spécifie une URL à appeler ainsi que des paramètres
- Deadline d'exécution de 30s pour chaque tache
- Possibilité de définir un « rate queue » spécifiant la rapidité à laquelle les tâches vont consommer les ressources

### Utilisation
On définit la queue sur le fichier queue.xml

```XML
<queue-entries>
 <queue>
  <name>default</name>
  <rate>1/s</rate>
 </queue>
 <queue>
  <name>mail-queue</name>
  <rate>2000/d</rate>
  <bucket-size>10</bucket-size>
 </queue>
 <queue>
  <name>background-processing</name>
  <rate>5/s</rate>
 </queue>
</queue-entries>
```

On fait notre task

```Java
String key="name";
String value="jdoe";
Queue queue = QueueFactory.getDefaultQueue();
// Ajout d’une tache simple
TaskOptions task=TaskOptions.Builder.withUrl("/worker").param(key, value);
queue.add(task);
// Ajout d’une tache simple avec des paramètres de configuration
Map<String, String> headers=new HashMap<String, String>();
headers.put("X-AppEngine-TaskName","task2");
headers.put("X-AppEngine-TaskRetryCount","4");
TaskOptions task2=TaskOptions.Builder.withUrl("/worker2").headers(headers);
queue.add(task2);
// Ajout d’une tache en spécifiant la méthode utilisée
TaskOptions
task3=TaskOptions.Builder.withUrl("/worker?a=b&c=d").method(Method.GET);
queue.add(task3);
//...
queue.deleteTask("task");
//...
queue.purge();
```

On doit ensuite planifier la tache à l'aide du fichier cron.xml

```XML
<?xml version="1.0" encoding="UTF-8"?>
<cronentries>
 <cron>
 <url>/recache</url>
 <description>Repopulate the cache every minutes</description>
 <schedule>every 2 minutes</schedule>
 </cron>
 <cron>
 <url>/weeklyreport</url>
 <description>Mail out a weekly report</description>
 <schedule>every monday 08:30</schedule>
 <timezone>America/New_York</timezone>
 </cron>
</cronentries>
```

Et pour finir on ajoute la sécurisation des URL des Tasks dans le web.xml

```XML
<security-constraint>
 <web-resource-collection>
 <url-pattern>/tasks/*</url-pattern>
 </web-resource-collection>
 <auth-constraint>
 <role-name>admin</role-name>
 </auth-constraint>
 </security-constraint>
```

## Service Channel
C'est un service qui permet de créer une connexion persistante entre les applications et les serveurs de google, permettant d'envoyer des messages et d'en recevoir.

**Propriétés**
- Crée une communication « socket like » entre server et client javascript
- Fourni une API javascript permettant de communiquer
- avec les channels
- Les serveurs reçoivent des mises à jour des données via HTTP
- Les clients javascript reçoivent des mises à jour des serveurs.

**Pour le code, voir le cours mais je pense pas que l'on en aura en DS**

## URL Fetch Service
C'est un service pour récupérer du contenu distant extérieur en appelant leurs URL. **Attention** : la connexion ne sera pas persistante.

**Propriétés**
- Support du protocole HTTP et HTTPs (vérification du certificat du serveur distant non assuré)
- Les autres protocoles ne sont pas supportés (e.g FTP)
- Support des actions HTTP Get-Post-Put-Head-Delete
- Support uniquement les connections standards TCP vers
- ports standards (HTTP 80,HTTPs 443)
- Appel Synchrone et Asynchrone possible

On peut l'utiliser de deux façon :
- Avec URL(openStream)
- Avec HttpURLConnection

On ne traitera dans l'exemple que la partie URL (plus simple ;) )

```Java
import java.net.MalformedURLException;
import java.net.URL;
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.IOException;
// …
 try {
 URL url = new URL("http://www.example.com/atom.xml");
BufferedReader reader = new BufferedReader(new
InputStreamReader(url.openStream()));
 String line;
 while ((line = reader.readLine()) != null) {
 // Traitement des données reçues
 }
 reader.close();
 } catch (MalformedURLException e) {
 // Gestion d’exceptions d’ouverture de flux
 } catch (IOException e) {
 // Gestion d’exceptions de lecture de flux
 }
```

## Service Mail
Ce service permet d'automatiser l'envoi et la reception de message avec l'extérieur.

**Propriétés**
- Les message reçus (messages/emails) sont perçus comme des POST actions par l'APP, similaire à une HTTP request
- Communication uniquement avec le protocole XMPP pour la messagerie instantanée
- Envoi et réception d'email avec pièces jointes possible
- Pour communiquer en chat (XMPP) le client doit se connecter à son application de chat (gtalk)
- Réception des email à l'adresse app-id@appspotmail.com ou anything@appid.appspotmail.com

### Envoi de mail

```Java
public class UserMailSender extends HttpServlet{
private static final long serialVersionUID = 1L;
@Override
protected void doGet(HttpServletRequest req, HttpServletResponse resp)
throws ServletException, IOException {
 Properties props = new Properties();
 Session session = Session.getDefaultInstance(props, null);
 String msgBody = "YO! Welcome to the JavaMail Service provided by Google App Engine !";
 try {
 Message msg = new MimeMessage(session);
 msg.setFrom(new InternetAddress("app@yahoo.fr", "Example.com","Admin"));
 msg.addRecipient(Message.RecipientType.TO,new InternetAddress("jDoe@cpe.fr", "Mr. User"));
 msg.setSubject("Your Example.com account has been activated");
 msg.setText(msgBody);
 Transport.send(msg);
 } catch (AddressException e) {e.printStackTrace();
 } catch (MessagingException e) {e.printStackTrace(); }
}
 }
```

### Reception de mail
configuration appengine-web.xml

```XML
<inbound-services>
 <service>mail</service>
</inbound-services>
```

Configuration du web.xml

```XML
<servlet>
 <servlet-name>mailhandler</servlet-name>
 <servlet-class>MailHandlerServlet</servlet-class>
</servlet>
<servlet-mapping>
 <servlet-name>mailhandler</servlet-name>
 <url-pattern>/_ah/mail/*</url-pattern>
</servlet-mapping>
<security-constraint>
 <web-resource-collection>
 <url-pattern>/_ah/mail/*</url-pattern>
 </web-resource-collection>
 <auth-constraint>
 <role-name>admin</role-name>
 </auth-constraint>
</security-constraint>
```

Code Java

```Java
public class MailHandlerServlet extends HttpServlet {
 public void doPost(HttpServletRequest req, HttpServletResponse resp)
 throws IOException {
 Properties props = new Properties();
 Session session = Session.getDefaultInstance(props, null);
 MimeMessage message;
 try {
 message = new MimeMessage(session, req.getInputStream());
 Address[] addresses = message.getFrom();
 if (message.isMimeType("text/plain")) {
 String content = (String)message.getContent();
 }
 } catch (MessagingException e) {e.printStackTrace();}
 }
}
```

# Auteur
- ([@Aveys](https://github.com/Aveys))
- et bientôt vous ;)
