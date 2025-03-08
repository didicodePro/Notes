### 📌 **Qu'est-ce que la logique métier ?**

👉 **La logique métier (business logic)**, c'est **l'ensemble des règles et traitements qui définissent comment fonctionne ton application**.
 Elle se trouve généralement dans le **Service** et non dans le Controller.

💡 **En gros, c'est "ce que fait ton application" au niveau des données.**

------

## 🎯 **Exemple simple : Une application de gestion d’événements**

- Un utilisateur peut **créer un événement**.
- Un événement ne peut **pas avoir un titre dupliqué**.
- Un événement **doit avoir une date valide et un organisateur**.

Ces règles définissent comment les événements doivent être **créés, modifiés ou supprimés** dans ton application.
 👉 **C'est de la logique métier !** 🚀

------

## **1️⃣ Différence entre Controller, Service et Modèle**

### ❌ **Mauvaise approche : Logique métier dans le Controller**

```ts
async createEvent(req: Request, res: Response) {
  try {
    // ❌ Le Controller ne devrait pas interagir directement avec la BDD
    const existingEvent = await EventModel.findOne({ title: req.body.title });
    if (existingEvent) {
      return res.status(400).json({ message: "Cet événement existe déjà" });
    }

    if (new Date(req.body.date) < new Date()) {
      return res.status(400).json({ message: "La date doit être dans le futur" });
    }

    const event = await EventModel.create(req.body);
    res.status(201).json(event);
  } catch (error) {
    res.status(500).json({ message: error.message });
  }
}
```

🔴 **Problèmes :**

- **Le Controller fait trop de choses** (requêtes BDD + règles de validation).
- **Impossible de réutiliser ces règles ailleurs** (ex: API GraphQL, script en arrière-plan).

------

### ✅ **Bonne approche : Logique métier dans le Service**

#### **Controller (`event.controller.ts`)**

```ts
async createEvent(req: Request, res: Response) {
  try {
    const event = await this.eventService.createEvent(req.body);
    res.status(201).json(event);
  } catch (error) {
    res.status(400).json({ message: error.message });
  }
}
```

👉 **Ici, le Controller ne fait que recevoir la requête et envoyer la réponse.**

#### **Service (`event.service.ts`)**

```ts
import EventModel from "../models/event.model.js";

export class EventService {
  async createEvent(eventData) {
    // 🔍 Logique métier : Vérifier si l'événement existe déjà
    const existingEvent = await EventModel.findOne({ title: eventData.title });
    if (existingEvent) {
      throw new Error("Cet événement existe déjà");
    }

    // 🔍 Logique métier : Vérifier que la date est valide
    if (new Date(eventData.date) < new Date()) {
      throw new Error("La date doit être dans le futur");
    }

    // ✅ Créer l'événement
    return await EventModel.create(eventData);
  }
}
```

✅ **Avantages :**

- **Le Controller est léger** et ne gère que la requête HTTP.
- **Le Service gère la logique métier** (validation, contrôle des doublons, règles de gestion).
- **Facile à tester et réutiliser** (ex: API REST, GraphQL, CRON jobs).

------

## **2️⃣ Exemples concrets de logique métier**

Voici d'autres **exemples de logique métier** selon les applications :

| **Application**                    | **Exemples de logique métier**                               |
| ---------------------------------- | ------------------------------------------------------------ |
| 🎟️ **Gestion d’événements**         | - Un utilisateur ne peut **pas s'inscrire deux fois** à un même événement.- Un événement doit être **dans le futur**.- Un organisateur peut **ajouter des participants**, mais **pas les supprimer** après un certain délai. |
| 🛍️ **E-commerce**                   | - Un client ne peut pas **acheter un produit en rupture de stock**.- Une commande ne peut pas être **annulée après expédition**.- Les utilisateurs VIP ont **-10% de réduction**. |
| 🎬 **Streaming (Netflix, Spotify)** | - Un film ne peut être vu que **si l’abonnement est actif**.- Un utilisateur ne peut pas **se connecter sur plus de 3 appareils en même temps**.- Certaines séries sont **bloquées selon la région**. |
| 🚖 **Uber / Bolt**                  | - Un chauffeur ne peut pas **accepter une nouvelle course s'il en a déjà une en cours**.- Une course doit être **payée avant de commencer**.- Un client ne peut **pas annuler une course après 5 minutes** sans pénalité. |

👉 **Toutes ces règles sont de la logique métier et doivent être dans le Service.**

------

## **🚀 Récapitulatif**

| Où mettre la logique ? | Que doit-il faire ?                                          |
| ---------------------- | ------------------------------------------------------------ |
| **Controller** ✅       | Gérer les requêtes et réponses HTTP (REST, GraphQL, WebSockets). |
| **Service** ✅          | Appliquer les **règles métier** (validation, permissions, calculs, restrictions). |
| **Modèle (`Model`)** ✅ | Définir la **structure des données** et interagir avec la BDD (CRUD). |

------

## **🎯 Conclusion**

✅ **La logique métier = toutes les règles qui définissent comment ton application fonctionne.**
 ✅ **Elle doit être mise dans le Service**, pas dans le Controller.
 ✅ **Le Controller ne gère que les requêtes/réponses HTTP**.
 ✅ **En séparant bien les responsabilités, ton code est plus propre, réutilisable et facile à maintenir.**

💡 **Tu veux un exemple de tests unitaires sur le Service pour vérifier la logique métier ?** 😃