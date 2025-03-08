### 🚀 **Est-ce que tout ce qu'on fait en MongoDB natif peut être fait avec Mongoose ?**

✅ **OUI**, mais avec quelques **différences** et **limitations**.

Mongoose est une **couche d’abstraction** au-dessus de MongoDB. Il facilite les requêtes en offrant :
 ✔ **Un modèle structuré** avec validation
 ✔ **Une syntaxe plus simple** pour les requêtes
 ✔ **Un système de middleware** et hooks
 ✔ **Un ORM (Object Relational Mapping)**

Mais il a aussi quelques **limitations** :
 ❌ **Moins performant pour les requêtes massives** (car il impose des modèles).
 ❌ **Certaines commandes MongoDB natives ne sont pas directement accessibles**.

------

# 🔥 **1️⃣ CRUD (Create, Read, Update, Delete)**

Tout ce qu’on fait en **MongoDB natif** peut être fait **avec Mongoose**.

### **✅ Insérer des données**

**MongoDB natif**

```js
db.users.insertOne({ name: "Alice", age: 28 });
```

**Mongoose**

```js
const newUser = new User({ name: "Alice", age: 28 });
await newUser.save();
```

------

### **✅ Lire des données**

**MongoDB natif**

```js
db.users.find({ age: { $gt: 25 } });
```

**Mongoose**

```js
await User.find({ age: { $gt: 25 } });
```

------

### **✅ Mettre à jour des données**

**MongoDB natif**

```js
db.users.updateOne({ name: "Alice" }, { $set: { age: 30 } });
```

**Mongoose**

```js
await User.updateOne({ name: "Alice" }, { age: 30 });
```

------

### **✅ Supprimer des données**

**MongoDB natif**

```js
db.users.deleteOne({ name: "Alice" });
```

**Mongoose**

```js
await User.deleteOne({ name: "Alice" });
```

------

# 🔍 **2️⃣ Recherches Avancées**

### **✅ Trier et Limiter**

**MongoDB natif**

```js
db.users.find().sort({ age: -1 }).limit(5);
```

**Mongoose**

```js
await User.find().sort({ age: -1 }).limit(5);
```

------

### **✅ Rechercher avec `$or`, `$and`**

**MongoDB natif**

```js
db.users.find({ $or: [{ age: { $lt: 25 } }, { role: "admin" }] });
```

**Mongoose**

```js
await User.find({ $or: [{ age: { $lt: 25 } }, { role: "admin" }] });
```

------

### **✅ Filtrer des champs spécifiques**

**MongoDB natif**

```js
db.users.find({}, { name: 1, email: 1, _id: 0 });
```

**Mongoose**

```js
await User.find().select("name email -_id");
```

------

# 🔥 **3️⃣ Les Agrégations (`aggregate()`)**

**Mongoose permet d’utiliser les agrégations MongoDB**, mais **pas aussi simplement que `find()`**.

### **✅ Compter les utilisateurs par rôle**

**MongoDB natif**

```js
db.users.aggregate([{ $group: { _id: "$role", total: { $sum: 1 } } }]);
```

**Mongoose**

```js
await User.aggregate([{ $group: { _id: "$role", total: { $sum: 1 } } }]);
```

💡 **Même syntaxe**, mais Mongoose doit passer par `.aggregate()`.

------

### **✅ Calculer la moyenne d’âge**

**MongoDB natif**

```js
db.users.aggregate([{ $group: { _id: null, avgAge: { $avg: "$age" } } }]);
```

**Mongoose**

```js
await User.aggregate([{ $group: { _id: null, avgAge: { $avg: "$age" } } }]);
```

------

# ⚠ **4️⃣ Ce qu’on ne peut PAS faire directement en Mongoose**

**Mongoose ne prend pas en charge certaines fonctionnalités MongoDB natives directement** :

### **❌ `createIndex()` (Création d'index directement)**

✔ MongoDB natif :

```js
db.users.createIndex({ email: 1 }, { unique: true });
```

❌ **Pas de méthode `createIndex()` en Mongoose**, mais on peut le faire via le schéma :

```js
const userSchema = new mongoose.Schema({
  email: { type: String, unique: true }
});
```

------

### **❌ `mapReduce()`**

✔ MongoDB natif :

```js
db.users.mapReduce(
  function () { emit(this.role, 1); },
  function (key, values) { return Array.sum(values); },
  { out: "role_counts" }
);
```

## ❌ **Mongoose ne supporte pas `mapReduce()` directement**.  💡 **Alternative : Utiliser `aggregate()` à la place.**

### **❌ Requêtes en Mode "Native"**

Si tu veux exécuter une requête 100% MongoDB **sans passer par Mongoose**, tu peux utiliser `User.collection`:

```js
await User.collection.insertOne({ name: "John", age: 30 });
```

💡 Cela permet d'accéder aux **méthodes MongoDB natives**.

------

# 🎯 **Conclusion**

| **Opération**                               | **MongoDB natif** | **Mongoose**          | **Compatible ?** |
| ------------------------------------------- | ----------------- | --------------------- | ---------------- |
| CRUD (Create, Read, Update, Delete)         | ✅                 | ✅                     | ✅                |
| Requêtes avancées (`$or`, `$and`, `$regex`) | ✅                 | ✅                     | ✅                |
| Agrégations (`aggregate()`)                 | ✅                 | ✅                     | ✅                |
| Indexation (`createIndex()`)                | ✅                 | ❌ (via schema)        | ⚠                |
| `mapReduce()`                               | ✅                 | ❌ (pas de support)    | ❌                |
| Accès natif                                 | ✅                 | ⚠ (`User.collection`) | ⚠                |

✅ **Tout ce qui est CRUD et requêtes classiques fonctionne bien avec Mongoose.**
 ✅ **Les agrégations aussi, mais elles sont moins intuitives que `find()`.**
 ❌ **Certaines commandes comme `mapReduce()` et `createIndex()` ne sont pas supportées directement.**

**Tu veux essayer une requête avancée en Mongoose ?** 😊🔥