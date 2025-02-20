###  Specification fonctionnel

ce que le clients devrait pouvoir faire

### Indentifier les ressources et le actions

**Ressources**
Vehicules

**Actions**
Creer / Create
Lire / Read
Mettre a jour / Update
Supprimer / Delete

### Principe de conception

**REST** (Representational State Transfer)
**SOAP** (Simple Object Acces Protocol)
**Graphql**

### Structure des requetes

Request , responses

3 Parties
**Verb**  
Action a effectuer sur le serveur 
(GET, POST, PUT, PATCH, DELETE)

**Headers** 
Metadonnee relatives a la demande
Content-Type : Le format du contenu
Content-Length : Taille du contenu
Autorisation : Qui passe l'appel
Acceptation : Quel(s) type peut etre accete
Cookies : Donnees du client dans la demande
plus d'en-tetes...

**Content**
Donnee a envoyer au serveur
HTML, CSS, Javascript, XML, JSON
Le contenu n'est pas valide avec certains verbes
Informations permettant de repondre a la demande
Binaires et blobs communs (Par exemple .jpg)

### Structure des responses

**Status Code**
(100-199) : Information
(200-299) : Succes
(300-399) : Redirection
(400-499) : Erreur du client
(500-599) : Erreurs du serveur

**Headers**
Metadonnee relatives a la demande
Content-Type : Le format du contenu
Content-Length : Taille du contenu
Autorisation : Qui passe l'appel
Acceptation : Quel(s) type peut etre accete
Cookies : Donnees du client dans la demande
plus d'en-tetes...

**Content**
HTML,CSS,Javascript, XML, JSON
Binaires et Blobs (par ex; jpg)
Les API on souvent leurs propres types



#### Principe REST

http backbone c'est le coeur de la mise en place de la comunication

#### Approche design first , URI et Verbs, L'idempotence (le POST n'est pas idempotent)



#### SEED DATA (Donnee pour les test)



