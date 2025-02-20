Super choix ! La **Clean Architecture** va rendre ton API **maintenable, testable et évolutive**. 🚀

------

### 🔹 **Plan pour créer une API en Clean Architecture avec Node.js + Express + MongoDB**

1. **Définir la structure du projet**
2. **Créer les entités (Domaine)**
3. **Implémenter les cas d'utilisation (Application)**
4. **Créer les adaptateurs (Interface : contrôleurs, DTOs)**
5. **Gérer l’infrastructure (Base de données, Frameworks, Serveur Express)**
6. **Ajouter les tests unitaires et d'intégration**

------

### **📁 Structure du projet**

```
/src
  /domain           # Modèles et Entités métiers
    - Task.js
  /application      # Cas d'utilisation
    - CreateTaskUseCase.js
    - GetTaskUseCase.js
  /infrastructure   # Services externes (DB, Frameworks, API tierces)
    - TaskRepository.js
    - MongoDBConnection.js
  /interface        # API REST (Controllers, DTOs)
    - TaskController.js
    - Routes.js
  /config           # Fichiers de configuration (env, logger, etc.)
  /tests            # Tests unitaires et d'intégration
  - server.js       # Point d'entrée de l'application
```

------

## 🔹 **1️⃣ Définition de l’entité "Task" (Domaine)**

L’entité doit être **indépendante de la base de données**.

```javascript
export default class Task {
  constructor(id, title, description, status = "to do") {
    this.id = id;
    this.title = title;
    this.description = description;
    this.status = status;
  }
}
```

------

## 🔹 **2️⃣ Cas d’utilisation (Application)**

Les **Use Cases** contiennent la logique métier.

### **Créer une tâche**

```javascript
export default class CreateTaskUseCase {
  constructor(taskRepository) {
    this.taskRepository = taskRepository;
  }

  async execute(taskData) {
    if (!taskData.title) {
      throw new Error("Le titre est requis.");
    }

    const task = new Task(null, taskData.title, taskData.description);
    return await this.taskRepository.save(task);
  }
}
```

### **Récupérer une tâche**

```javascript
export default class GetTaskUseCase {
  constructor(taskRepository) {
    this.taskRepository = taskRepository;
  }

  async execute(taskId) {
    return await this.taskRepository.findById(taskId);
  }
}
```

------

## 🔹 **3️⃣ Repository (Accès aux données - Infrastructure)**

Ce fichier contient l’interface avec MongoDB.

```javascript
import TaskModel from "../models/task.model.js";
import Task from "../domain/Task.js";

export default class TaskRepository {
  async save(task) {
    const newTask = await TaskModel.create(task);
    return new Task(newTask._id, newTask.title, newTask.description, newTask.status);
  }

  async findById(id) {
    const task = await TaskModel.findById(id);
    return task ? new Task(task._id, task.title, task.description, task.status) : null;
  }
}
```

------

## 🔹 **4️⃣ Contrôleur (Interface - API REST)**

Ici, on relie l’API aux **cas d’utilisation**.

```javascript
import CreateTaskUseCase from "../application/CreateTaskUseCase.js";
import GetTaskUseCase from "../application/GetTaskUseCase.js";
import TaskRepository from "../infrastructure/TaskRepository.js";

const taskRepository = new TaskRepository();
const createTaskUseCase = new CreateTaskUseCase(taskRepository);
const getTaskUseCase = new GetTaskUseCase(taskRepository);

export const createTask = async (req, res) => {
  try {
    const task = await createTaskUseCase.execute(req.body);
    res.status(201).json({ success: true, data: task });
  } catch (error) {
    res.status(400).json({ success: false, error: error.message });
  }
};

export const getTask = async (req, res) => {
  try {
    const task = await getTaskUseCase.execute(req.params.id);
    if (!task) return res.status(404).json({ success: false, error: "Tâche non trouvée" });

    res.status(200).json({ success: true, data: task });
  } catch (error) {
    res.status(500).json({ success: false, error: error.message });
  }
};
```

------

## 🔹 **5️⃣ Routes (API Express)**

On définit les routes de l’API.

```javascript
import express from "express";
import { createTask, getTask } from "./TaskController.js";

const router = express.Router();

router.post("/tasks", createTask);
router.get("/tasks/:id", getTask);

export default router;
```

------

## 🔹 **6️⃣ Serveur Express (server.js)**

```javascript
import express from "express";
import mongoose from "mongoose";
import router from "./interface/Routes.js";
import dotenv from "dotenv";

dotenv.config();

const app = express();
app.use(express.json());
app.use("/api", router);

const PORT = process.env.PORT || 5000;

mongoose
  .connect(process.env.MONGO_URI, { useNewUrlParser: true, useUnifiedTopology: true })
  .then(() => {
    console.log("MongoDB connecté !");
    app.listen(PORT, () => console.log(`Serveur en écoute sur le port ${PORT}`));
  })
  .catch((error) => console.error("Erreur de connexion à MongoDB:", error));
```

------

## ✅ **Pourquoi cette approche est puissante ?**

✔️ **Séparation des responsabilités** : Chaque couche a son rôle.
 ✔️ **Facilement testable** : Les cas d’utilisation peuvent être testés indépendamment.
 ✔️ **Évolutif** : On peut changer de base de données sans impacter le domaine.
 ✔️ **Respecte les principes SOLID**.

------

### 🎯 **Prochaine étape :**

- Ajouter **d’autres cas d'utilisation** (mise à jour, suppression).
- Ajouter **les tests unitaires et d’intégration** (Jest + Supertest).
- Mettre en place une **documentation OpenAPI (Swagger)**.

Tu veux un guide pour les tests unitaires avec cette architecture ? 🚀