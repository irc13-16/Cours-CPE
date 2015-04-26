TLI CheatSheet
==================
Ce document ne résume pas l'intégralité du cours, seulement les points qui me semble important et/ou oublié.

Les base du web
---------
URL : Uniform Resource Locator

### HTTP
4 types de requêtes HTTP :
* GET
* POST
* PUT
* DELETE

### Langages
#### HTML
* Caracteristiques du XHTML:
    - Respect du format XML (stricte)
    - balises de fermeture obligatoire
    - tous les attributs doivent etre valuée et entre guillemets
    - sensible à la case: tout en minuscule

### Accessibilité
*définition :* Un site internet est dit accessible si tout utilisateur, quelques soient sa
configuration
matérielle et sa configuration logicielle, est capable d’apprécier le contenu de ce site
de la même manière que tout autre utilisateur pourvu de configurations différentes.

**Avantages :** 
* modélisation robuste
* garantie de portabilité et d'adaptation
* facilité d'évolution
* Aspect politique et com

**Inconvénients :** 
* convaincre les décideurs et négocier avec les webdesigners
* à envisager dés le début du projet
* conception + longue

### Conception d'un site web accessible
1. analyse
    * identifier le contexte (charte graphique existente, public visé, ...)
    * identifier les objectfs (contraintes et niveau d'accessibilité)
    * Identifier les moyens (qui fait quoi, teste, en combien de temps)
2. préparation
    * Collecter, préparer et organiser les ressources 
    * Realiser des concepts d'interface
    * Identifier le contenu et la forme
3. contenu
    * structurer
    * saisir
    * adapter
4. Mise en forme (Le CSS, pas de style dans le HTML)
5. Tests et déploiements
    * Tester est obligatoire en fonctionnel et sécurité
    * valider le code html, css et donc l'accessibilité

Le web dynamique
---------
*définition :* modifier les informations et contenu qu'une page présente par rapoport a des etats interne et externe
### coté serveur
*CGI :* Common Gateway interface. éxécuté par le serveur HTTP
*Active Server Page (ASP, PHP) :* langage interprété par un compilateur lié au serveur http
#### CGI
* Mecanisme simple et universel
* indépendance du systéme d'exploitation
* lance un process pour chaque requête -> lourd
#### PHP
* + rapide
* plus de fonctionnalités
* modéle Objet
* Quelques incompatibilités
* Des problémes de sécurité
    - accés partiels au fichiers et commandes systéme
    - initialisation des variables obligatoire
    - toujours vérifier les variables d'inputs

PHP avancé
------------
### les sessions
* permet de faire une pseudo-connexion
* optimisé et facile à utiliser
* Fonctionnement : 
    1. Session_start() -> genere un SESSID unique transmis au client par un cookie
    2. stockage dans un tableau lié au SESSID coté serveur de variables
    3. Chaque nouvelle requete client donne lieu a un transfert du SESSID vers le serveur
    4. la sessions est détruite aprés un certain temps ou deconnexion manuelle

### PDO
**Avantages de PDO**
* PDO est objet
* gere les exeptions
* marche avec toute BDD
* preparation des requqtes 
* protege des injection

XML et définition
----------
### XML
* permet de représenter des données de façon spécifique et arborescente
* Une API existe pour presque tous les langages
* Un document XML est valide si :
    - Il est valide dans la forme (respect de la syntaxe)
    - Il est valide dans le fond (respecte un modéle : ne pas mettre de texte dans un champ de nombres)
* Deux solutions pour donner un modéle : 
    - DTD: Document Type Declaration (Doctype)
    - XSD: XML Schema Declaration (namespace)
    - ATTENTION : ne pas faire la même sémantique que HTML

### DTD
| **Symbole**                   | **Description**                                                                     | **Exemple**                                                                          |
|--------------------------|-----------------------------------------------------------------------------|----------------------------------------------------------------------------------|
| #PCDATA                  | Contains parsed character data or text                                      | <element (#PCDATA)>                                                              |
| #PCDATA, element-name    | Contains text and another element; #PCDATA is always listed first in a rule | <element (#PCDATA, child)*>                                                      |
| , (comma)                | Must use in this order                                                      | <element (child1, child2, child3)>                                               |
| | (pipe bar)             | Use only one element of the choices provided                                | <element (child1 | child2 | child3)>                                             |
| element-name (by itself) | Use one time only                                                           | <element (child)>                                                                |
| element-name?            | Use either once or not at all                                               | <element (child1, child2?, child3?)>                                             |
| element-name+            | Use either once or many times                                               | <element (child1+, child2?, child3)>                                             |
| element-name*            | Use once, many times, or not at all                                         | <element (child1*, child2+, child3)>                                             |
| ( )                      | Indicates groups; may be nested                                             | <element (#PCDATA | child)*> or <element ((child1*, child2+, child3)* | child4)> |
#### Exemple
```XML
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<!DOCTYPE liste_de_gens [
 <!ELEMENT liste_de_gens (personne)*>
 <!ELEMENT personne (nom, date_de_naissance?, genre?, numero_de_secu?)>
 <!ELEMENT nom (#PCDATA)>
 <!ELEMENT date_de_naissance (#PCDATA)>
 <!ELEMENT genre (#PCDATA | masculin | féminin) "féminin">
 <!ELEMENT numero_de_secu (#PCDATA)>
]>
<liste_de_gens>
  <personne>
    <nom>Fred Bloggs</nom>
    <date_de_naissance>2008-11-27</date_de_naissance>
    <genre>masculin</genre>
  </personne>
</liste_de_gens>
```
### XSD
| **Nom**    | **But**                                                                                                             | **Syntaxe**                                                    |
|---------------------|---------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------|
| Schema              | Identifies the language the schema uses                                                                             | <xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"> |
| Element             | Defines an element                                                                                                  | <xsd:element name="name">                                 |
| Attribute           | Defines an attribute                                                                                                | <xsd:attribute name="name" type="type">                   |
| Complex type        | Defines an element that contains other elements, contains attributes, or contains mixed content (elements and text) | <xsd:complexType>                                         |
| Simple type         | Creates a constrained datatype for an element or attribute value                                                    | <xsd:simpleType>                                          |
| Sequence compositor | Specifies that attributes or elements within a complex type must be listed in order                                 | <xsd:sequence>                                            |
* Quand on utilise un namespace alias (xxx:eee), il n’est valable que pour la balise et pas ses enfants. Sinon on redéfinit le namespace avec xmlns pour conserver l'héritage
* Toujours faire un element root et ensuite des complexType

#### Exemple
```XML
<?xml version="1.0" encoding="UTF-8"?>
  <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">
    <xs:element name="personne">
      <xs:complexType>
        <xs:sequence>
          <xs:element name="nom" type="xs:string" />
          <xs:element name="prenom" type="xs:string" />
          <xs:element name="date_naissance" type="xs:date" />
          <xs:element name="etablissement" type="xs:string" />
          <xs:element name="num_tel" type="xs:string" />
        </xs:sequence>
      </xs:complexType>
    </xs:element>
  </xs:schema>
```

```XML
 <?xml version="1.0" encoding="UTF-8"?>
  <personne xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="personne.xsd">
    <nom>MBODJ</nom>
    <prenom>Babacar</prenom>
    <date_naissance>1996-10-06</date_naissance>
    <etablissement>NIIT</etablissement>
    <num_tel>764704140</num_tel>
  </personne>
```

### Différence DTD/XSD

* les schemas sont en XML et vont pouvoir gerer les namespace contrairement au DTD
* Les XSD sont en XML donc parsable et verifier leur schéma (verifier validité et syntaxe d'un XSD)
* La DTD défini le XML(XSD) qui définit les XML
* les schema sont plus précis dans les cardinalités
* XSD -> héritage
* typage des attributs des balises dans le schema
* XSD facilement utilisable (en totalité ou en partie)

XSL
------

* transformation d'un document XML en tout ce que l'on veut
* c'est un langage en XML et une techno rare car on ne le voit pas
* XSL est pertinent chaque fois qu'on a besoin de transformer un fichier XML et pour représenter des données
* Beaucoup de parser mais n'implementant pas forcément la v2
    - SAXON est la référence
* systéme de template par selecteurs
* variables non modifiables
* plusieurs modes de sortie possible : HTML, XML, text
* XSL -> XSLT et XSL-FO

### Selecteurs

**VOIR LE COURS XML ET XSL SLIDE 22-23**
Ne pas oublier le selecteur Self pour XPATH

###Instructions

Un template match n'est evalué qu'au début ou quand on le demande explicitement
* Les variables ne sont pas modifiable en  XSL

#### Comment écrire du texte ?
Ecriture directe sauf si 
* Tag dans le langage cible
* <xsl:texte> </xsl:text>
* valueof select="..." -> chaine de caractére (TOUJOURS)
* copy-of select="noeud" -> fragment (noeud+ascendant) (bloc de code)
* <copy>    </copy>

#### XSL2

as="" -> typage , ? -> ce type ou rien

Les parseurs et Java
-----

Important : 2 types d'API différentes
**Avantages :**
* messages d’erreurs plus clairs
* le document xml doit être valide sous tout rapport !
* il existe des outils de "nettoyage" comme tagsoup par exemple (http ://home.ccil.org/ cowan/tagsoup/) pour sax
* les processus de parsage et d’analyse sont finement configurables (validation, paramètres)
* on peut les utiliser dynamiquement (variable pour des paramètres xsl par exemple)
* on peut décider comment gérer les erreurs (génération et traitement des exeptions)

###DOM

* Crée un arbre proche du document xml dans la mémoire,
* Assez lourd en mémoire,
* trés orienté objet
* Connaitre le graphique objet DOM (**Cours API/PARSER Slide 10**)

###SAX

* SAX => Approche evenementielle, 
* SAX et DOM non compatible, 
* permet de parser facilement

### DP Fabrique

**voir cours API/PARSER slide 20**

WebServices et REST
------
**Principe:**
* Encapsuler des services réseaux sur le web (passge par Port 80, donner accés à des données)
* Délégation des compétences (*je demande a qqun qui fait mieux que moi*)
* Dialogue entre machines

**2 approches**
* Client-Serveur (historique)
* Agents-Ressources

### WSDL
Format de description des échange en XML. A utiliser avec SOAP ou UDDI

### REST
* Une URL est lié à son service
* Fonctionne avec HTTP principalement avec les 4 mots-clés (GET, POST, PUT, DELETE)
