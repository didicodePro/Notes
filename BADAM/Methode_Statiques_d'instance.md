### 📌 **Méthodes statiques vs Méthodes d'instance en Mongoose**

En **Mongoose**, on peut définir deux types de méthodes sur un modèle **MongoDB** :
 1️⃣ **Méthodes statiques** → **Fonctions disponibles sur le modèle** (`Model`)
 2️⃣ **Méthodes d'instance** → **Fonctions disponibles sur un document** (`Document`)

------

## 🏗️ **1. Méthodes statiques (`statics`)**

👉 **Les méthodes statiques sont attachées directement au modèle (`Model`).**
 👉 Elles permettent d'effectuer des opérations **globales** sur la base de données.

### **💻 Exemple : Trouver toutes les tâches "done"**

```js
import mongoose from "mongoose";

const taskSchema = new mongoose.Schema({
  name: String,
  status: { type: String, enum: ["pending", "in-progress", "done"] },
});

// Méthode statique
taskSchema.statics.findCompleted = function () {
  return this.find({ status: "done" }); // `this` fait référence au modèle TaskModel
};

export const TaskModel = mongoose.model("Task", taskSchema);
```

#### **📌 Utilisation d'une méthode statique**

```js
const completedTasks = await TaskModel.findCompleted();
console.log(completedTasks);
```

🔹 **Pourquoi utiliser une méthode statique ?**
 ✅ Quand on veut **travailler sur plusieurs documents** en même temps
 ✅ Quand l’opération concerne **tout le modèle** (ex: `find`, `count`, `aggregate`)

------

## 🏗️ **2. Méthodes d'instance (`methods`)**

👉 **Les méthodes d'instance sont attachées à un document (`document`) spécifique.**
 👉 Elles permettent d'exécuter des actions **sur un seul document à la fois**.

### **💻 Exemple : Marquer une tâche comme "done"**

```js
const taskSchema = new mongoose.Schema({
  name: String,
  status: { type: String, enum: ["pending", "in-progress", "done"] },
});

// Méthode d'instance
taskSchema.methods.markAsDone = function () {
  this.status = "done"; // `this` fait référence à un seul document
  return this.save(); // Sauvegarde le document mis à jour
};

export const TaskModel = mongoose.model("Task", taskSchema);
```

#### **📌 Utilisation d'une méthode d'instance**

```js
const task = await TaskModel.findById("65a7e34f4b4e9d1234567890"); // Un seul document
await task.markAsDone(); // Met à jour cette tâche spécifique
console.log(task);
```

🔹 **Pourquoi utiliser une méthode d'instance ?**
 ✅ Quand on veut **modifier un document unique**
 ✅ Quand on doit **manipuler les données d’un document avant de les sauvegarder**

------

## 🆚 **Comparaison rapide : Quand utiliser quoi ?**

| Type de méthode         | Sur quoi ça s'applique                | Utilisation typique                                   | Exemples                                     |
| ----------------------- | ------------------------------------- | ----------------------------------------------------- | -------------------------------------------- |
| **Méthodes statiques**  | **Sur tout le modèle (`Model`)**      | Quand on veut agir sur **plusieurs documents**        | `findAll`, `countDocuments`, `aggregate`     |
| **Méthodes d'instance** | **Sur un seul document (`Document`)** | Quand on veut modifier **une seule donnée** à la fois | `markAsDone`, `calculateAge`, `updateStatus` |

------

## 🎯 **Exemple combiné : Static + Instance**

```js
const taskSchema = new mongoose.Schema({
  name: String,
  status: { type: String, enum: ["pending", "in-progress", "done"] },
});

// Méthode statique → Trouver toutes les tâches "done"
taskSchema.statics.findCompleted = function () {
  return this.find({ status: "done" });
};

// Méthode d'instance → Marquer une tâche comme "done"
taskSchema.methods.markAsDone = function () {
  this.status = "done";
  return this.save();
};

export const TaskModel = mongoose.model("Task", taskSchema);
```

#### **📌 Utilisation**

```js
// Utilisation de la méthode statique
const completedTasks = await TaskModel.findCompleted();
console.log(completedTasks);

// Utilisation de la méthode d'instance
const task = await TaskModel.findById("65a7e34f4b4e9d1234567890");
await task.markAsDone();
console.log(task);
```

------

## 🎯 **Conclusion**

✅ **Méthodes statiques (`statics`)** → Quand on veut agir **sur tout le modèle** (ex: `find`, `count`)
 ✅ **Méthodes d'instance (`methods`)** → Quand on veut **modifier un document spécifique**

💡 **Si tu veux appliquer une action sur plusieurs documents ➝ Méthode statique**
 💡 **Si tu veux modifier une seule donnée à la fois ➝ Méthode d'instance**

Tu veux un exemple plus avancé avec des hooks Mongoose (`pre`, `post`) ? 😊