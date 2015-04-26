Format XML
===========
XML est valide dans la forme = abc jop hup est valide en français (bonne lettres).
c'est le langage qui donne ça mais ca veut rien dire : la forme n'est pas valide.
La forme doit respecter un modéle.
il faut toujours vérifier les deux.
question document valide ? => verifier si le modéle est là pour savoir si c'est valide

Différence entre DTD et schéma :
* les schemas sont en XML et vont pouvoir gerer les namespace contrairement au DTD
* Les XSD sont en XML donc parsable et verifier leur schéma (verifier validité et syntaxe d'un XSD)
* La DTD défini le XML(XSD) qui définit les XML
* les schema sont plus précis dans les cardinalités
* typage des attributs des balises dans le schema
* XSD facilement utilisable (en totalité ou en partie)

DTD
---------
* se déclare avec le doctype (PUBLIC ou SYSTEM)
* xmlns : <<RL> -> valable pour tous les enfants
* ns:math (les alias en gros) -> valable uniquement pour la balise

XSD
------------
* toujurs faire un seul element racine de type XXXX, ensuite faire des types complexes

XSL
===========
*transformation d'un document XML en tout ce que l'on veut*
c'est un langage en XML et une techno rare car on ne le voit pas
XSL est pertinent chaque fois qu'on a besoin de transformer un fichier XML et pour représenter des données


Selecteurs
-------------
Ne pas oublier le selecteur Self pour XPATH

Instructions
----------------
Un template match n'est evalué qu'au début ou quand on le demande explicitement
* Les variables ne sont pas modifiable en  XSL

Comment écrire du texte 
* Ecriture directe sauf si 
* Tag dans le langage cible
* <xsl:texte> </xsl:text>
* valueof select="..." -> chaine de caractére (TOUJOURS)
* copy-of select="noeud" -> fragment (noeud+ascendant) (bloc de code)
* <copy>    </copy>

XSL2

as="" -> typage , ? -> ce type ou rien

Les parseurs
===========

Important : 2 types d'API différents

DOM
--------
* Crée un arbre proche du document xml dans la mémoire,
* Assez lourd en mémoire,
* trés orienté objet
* Connaitre le graphique objet DOM

SAX
------------
* SAX => Approche evenementielle, 
* SAX et DOM non compatible, 
* permet de parser facilement