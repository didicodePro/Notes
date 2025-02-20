Une **API RESTful** est une **interface de programmation d'application (API)** qui respecte les principes de l'architecture REST (**Representational State Transfer**). Le terme **RESTful** est utilisé pour indiquer que l'API suit **strictement les principes de REST**, qui sont conçus pour être simples, évolutifs et performants.

---

### **Principes d'une API RESTful**
Pour comprendre pourquoi une API est qualifiée de RESTful, voici les principes fondamentaux qu'elle doit respecter :

---

#### **1. Architecture client-serveur**
- L'API RESTful repose sur une **séparation claire des responsabilités** :
  - **Client** : Interface utilisateur (ex. frontend, application mobile).
  - **Serveur** : Gère les données et la logique métier (ex. backend).
- Cette séparation permet une indépendance entre les deux, rendant l'application plus évolutive.

---

#### **2. Communication sans état (stateless)**  
- Chaque requête HTTP d'une API RESTful est **indépendante** et contient toutes les informations nécessaires pour être traitée (par ex. un token pour l'authentification).
- Le serveur ne stocke pas l'état des clients entre les requêtes.

👉 Exemple :
- Lorsqu'un client envoie une requête pour récupérer un contact (`GET /contacts/123`), l'API RESTful ne se souvient pas des requêtes précédentes. Le serveur traite cette requête indépendamment.

---

#### **3. Représentation des ressources**
- Une **ressource** (données manipulées par l'API, comme un utilisateur ou un article) est représentée par une **URI** (Uniform Resource Identifier), généralement dans un format clair et lisible.
- Les ressources sont manipulées via des **verbes HTTP** (GET, POST, PUT, DELETE, etc.).

👉 Exemple :
- Une ressource `User` peut être représentée comme ceci :
  - `GET /users` : Récupérer tous les utilisateurs.
  - `GET /users/1` : Récupérer un utilisateur spécifique avec l'ID `1`.
  - `POST /users` : Créer un nouvel utilisateur.
  - `DELETE /users/1` : Supprimer l'utilisateur avec l'ID `1`.

---

#### **4. Utilisation des verbes HTTP**
- Les **verbes HTTP** (aussi appelés méthodes) sont utilisés pour indiquer l'intention de la requête.
- Une API RESTful attribue un rôle précis à chaque verbe :
  - **GET** : Lire une ressource.
  - **POST** : Créer une nouvelle ressource.
  - **PUT** : Modifier une ressource existante.
  - **DELETE** : Supprimer une ressource.

👉 Exemple :
- Pour manipuler une ressource "Article" :
  - `GET /articles` → Récupérer tous les articles.
  - `POST /articles` → Ajouter un nouvel article.
  - `PUT /articles/5` → Modifier l'article avec l'ID `5`.
  - `DELETE /articles/5` → Supprimer l'article avec l'ID `5`.

---

#### **5. Représentation multiple**
- Une API RESTful peut renvoyer les données dans différents formats en fonction des besoins du client (souvent JSON ou XML).
- Le client spécifie généralement le format souhaité à l'aide du header **`Accept`** dans la requête HTTP.

👉 Exemple :
- Une requête avec `Accept: application/json` renvoie les données en JSON :
  ```json
  {
    "id": 1,
    "name": "John Doe",
    "email": "john.doe@example.com"
  }
  ```

---

#### **6. Hypermedia comme moteur d'état (HATEOAS)**  
- Une API RESTful doit inclure des liens hypertexte dans ses réponses pour guider les clients sur les actions possibles.
- Cela permet de rendre l'API **auto-descriptive**.

👉 Exemple :
- Réponse d'une requête `GET /users/1` :
  ```json
  {
    "id": 1,
    "name": "John Doe",
    "email": "john.doe@example.com",
    "_links": {
      "self": { "href": "/users/1" },
      "update": { "href": "/users/1", "method": "PUT" },
      "delete": { "href": "/users/1", "method": "DELETE" }
    }
  }
  ```

---

### **Pourquoi "RESTful" ?**

Une API est qualifiée de **RESTful** lorsqu’elle respecte strictement ces principes REST. Cela signifie que :

1. **Elle traite les ressources correctement :**
   - Les ressources sont accessibles via des URI clairs.
   - Les verbes HTTP sont utilisés de manière cohérente.
2. **Elle est stateless :**
   - Le serveur ne conserve pas l’état entre les requêtes.
3. **Elle est auto-descriptive :**
   - Les réponses contiennent des informations et des liens pour guider les clients.
4. **Elle est évolutive et flexible :**
   - L'architecture RESTful favorise une communication standardisée et indépendante entre client et serveur.

Si une API ne respecte pas tous ces principes (par exemple, si elle utilise un seul endpoint `/api` avec toutes les actions regroupées dedans), elle ne peut pas être qualifiée de "RESTful".

---

### **Exemple d'une API RESTful complète**

#### **Ressource : `Contact`**

| Action                        | Verbe HTTP | URI               | Description                         |
|-------------------------------|------------|-------------------|-------------------------------------|
| Créer un contact              | POST       | `/contacts`       | Ajouter un nouveau contact          |
| Récupérer tous les contacts   | GET        | `/contacts`       | Obtenir la liste des contacts       |
| Récupérer un contact spécifique | GET        | `/contacts/1`     | Obtenir le contact avec l'ID `1`    |
| Modifier un contact           | PUT        | `/contacts/1`     | Modifier le contact avec l'ID `1`   |
| Supprimer un contact          | DELETE     | `/contacts/1`     | Supprimer le contact avec l'ID `1`  |

---

### **Résumé**
- **REST** est un style architectural basé sur les principes de simplicité, évolutivité et standardisation.
- Une API **RESTful** est une API qui suit strictement ces principes.
- Le terme **"RESTful"** met l'accent sur le respect des conventions et des bonnes pratiques REST.

Si tu veux approfondir un point spécifique, n'hésite pas ! 😊