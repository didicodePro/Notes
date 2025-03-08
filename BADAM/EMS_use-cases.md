Voici toutes les routes API possibles en fonction des rôles (**participant, organisateur, admin**) et des relations entre les entités (**Users, Events, Registration, Comments**).

------

## **🔹 Authentification**

### **🔹 Routes publiques**

- `POST /auth/register` → Inscription (Participant par défaut)
- `POST /auth/login` → Connexion
- `POST /auth/logout` → Déconnexion
- `GET /auth/me` → Récupérer les infos de l'utilisateur connecté

------

## **🔹 Utilisateurs (Users)**

### **🔹 Admin**

- `GET /users` → Récupérer tous les utilisateurs
- `GET /users/:id` → Récupérer un utilisateur spécifique
- `PUT /users/:id` → Modifier un utilisateur (admin)
- `DELETE /users/:id` → Supprimer un utilisateur

### **🔹 Utilisateur connecté**

- `PATCH /users/:id` → Modifier ses propres informations

------

## **🔹 Événements (Events)**

### **🔹 Tous les utilisateurs**

- `GET /events` → Récupérer tous les événements
- `GET /events/:id` → Récupérer un événement spécifique

### **🔹 Organisateur**

- `POST /events` → Créer un événement (devient organisateur)
- `PUT /events/:id` → Modifier un événement créé par soi-même
- `DELETE /events/:id` → Supprimer un événement créé par soi-même

### **🔹 Admin**

- `DELETE /events/:id` → Supprimer n'importe quel événement

------

## **🔹 Inscriptions aux événements (Registration)**

### **🔹 Participant**

- `POST /registrations` → S'inscrire à un événement
- `DELETE /registrations/:id` → Annuler son inscription

### **🔹 Organisateur**

- `GET /registrations/event/:eventId` → Voir les inscriptions à son événement
- `PATCH /registrations/:id/status` → Accepter ou refuser une inscription

### **🔹 Admin**

- `DELETE /registrations/:id` → Supprimer une inscription

------

## **🔹 Commentaires (Comments)**

### **🔹 Tous les utilisateurs**

- `GET /comments/event/:eventId` → Voir les commentaires d’un événement

### **🔹 Participant inscrit à un événement**

- `POST /comments/event/:eventId` → Ajouter un commentaire

### **🔹 Auteur du commentaire**

- `PUT /comments/:id` → Modifier son propre commentaire
- `DELETE /comments/:id` → Supprimer son propre commentaire

### **🔹 Organisateur ou Admin**

- `DELETE /comments/:id` → Supprimer n'importe quel commentaire

------

## **🔹 Administration (Admin)**

- `GET /dashboard` → Voir les statistiques générales
- `PATCH /users/:id/role` → Modifier le rôle d'un utilisateur

------

Si tu veux des ajustements ou des routes supplémentaires, dis-moi 😎🔥



Bonne remarque ! 👍 Je vais t'expliquer la logique pour garder une convention claire et cohérente.

### **🛠️ Règle pour nommer les paramètres d'URL**

1. **Si l'ID est lié directement à la ressource principale de la route**, on utilise **`:id`**
   - Exemple : `/events/:id` (on récupère un événement spécifique)
   - Exemple : `/users/:id` (on récupère un utilisateur spécifique)
2. **Si l'ID fait référence à une relation avec une autre ressource**, on utilise **le nom de la ressource + "Id"**
   - Exemple : `/registrations/event/:eventId` (on récupère les inscriptions d’un événement spécifique)
   - Exemple : `/comments/event/:eventId` (on récupère les commentaires liés à un événement)

### **🛠️ Application des conventions**

- `/events/:id` ✅ (Un événement spécifique)
- `/registrations/event/:eventId` ✅ (Les inscriptions d’un événement spécifique)
- `/comments/event/:eventId` ✅ (Les commentaires d’un événement spécifique)
- `/users/:id` ✅ (Un utilisateur spécifique)

### **🔥 Conclusion**

- Quand la route concerne **une seule entité**, on garde **`:id`**.
- Quand la route concerne **une relation avec une autre entité**, on précise **le nom de l'entité + "Id"**.

Je vais corriger toutes les routes en appliquant cette convention 👇

------

## **Routes API corrigées** ✅

### **🔹 Événements (Events)**

- `GET /events` → Récupérer tous les événements
- `GET /events/:id` → Récupérer un événement spécifique
- `POST /events` → Créer un événement (devient organisateur)
- `PUT /events/:id` → Modifier un événement créé par soi-même
- `DELETE /events/:id` → Supprimer un événement créé par soi-même

### **🔹 Inscriptions aux événements (Registration)**

- `POST /registrations` → S'inscrire à un événement
- `DELETE /registrations/:id` → Annuler son inscription
- `GET /registrations/event/:eventId` → Voir les inscriptions à un événement
- `PATCH /registrations/:id/status` → Accepter ou refuser une inscription

### **🔹 Commentaires (Comments)**

- `GET /comments/event/:eventId` → Voir les commentaires d’un événement
- `POST /comments/event/:eventId` → Ajouter un commentaire
- `PUT /comments/:id` → Modifier son propre commentaire
- `DELETE /comments/:id` → Supprimer son propre commentaire

**Maintenant tout est cohérent !** 🔥🚀
 Dis-moi si tu veux encore des ajustements ! 😎