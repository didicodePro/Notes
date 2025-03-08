Voici une liste des **meilleures conventions à respecter** lorsque tu travailles avec **Mongoose** :

------

## 🔹 **1. Nom des modèles**

✅ **Singulier & PascalCase** (première lettre en majuscule)
 🚫 **Pas de pluriel, pas de camelCase, pas de snake_case**

✔ **Bon :**

```js
const User = mongoose.model("User", userSchema);
const Movie = mongoose.model("Movie", movieSchema);
```

❌ **Mauvais :**

```js
const Users = mongoose.model("Users", userSchema);  // Mauvais : pluriel
const movie = mongoose.model("movie", movieSchema); // Mauvais : minuscule
const user_model = mongoose.model("user_model", userSchema); // Mauvais : snake_case
```

📌 **Pourquoi ?**

- Mongoose crée automatiquement une **collection en pluriel** (`users`, `movies`, etc.).
- Un modèle représente **une seule entité**, donc on le met au singulier.

------

## 🔹 **2. Nom des collections (automatique)**

✅ **Toujours en minuscule et au pluriel** (Mongoose gère ça tout seul)
 ❌ Ne pas spécifier le nom en dur, sauf si nécessaire

✔ **Bon :**

```js
const Product = mongoose.model("Product", productSchema); // Collection => "products"
```

❌ **Mauvais :**

```js
const Product = mongoose.model("Product", productSchema, "ProductList"); // Mauvais si pas nécessaire
```

------

## 🔹 **3. Nom des champs dans le schéma**

✅ **camelCase pour les objets JS**
 ✅ **snake_case si la base de données est utilisée ailleurs (optionnel)**

✔ **Bon :**

```js
const userSchema = new mongoose.Schema({
  firstName: String, // camelCase
  lastName: String,
  email: String,
  createdAt: { type: Date, default: Date.now }
});
```

❌ **Mauvais :**

```js
const userSchema = new mongoose.Schema({
  first_name: String, // Mauvais : snake_case en JS
  LastName: String,   // Mauvais : PascalCase
  Email: String       // Mauvais : première lettre en majuscule
});
```

📌 **Pourquoi ?**

- **camelCase** est la convention standard en **JavaScript**.
- Si tu utilises la base de données **dans un autre langage** (ex : Python, PHP), **snake_case** peut être une option.

------

## 🔹 **4. Références (`ref`) et relations entre modèles**

✅ **Utiliser le nom exact du modèle**
 ✅ **Utiliser `ObjectId` pour les références**
 ✅ **Mettre `required: true` si la relation est obligatoire**

✔ **Bon :**

```js
const commentSchema = new mongoose.Schema({
  text: String,
  user: { type: mongoose.Schema.Types.ObjectId, ref: "User", required: true },
  movie: { type: mongoose.Schema.Types.ObjectId, ref: "Movie", required: true },
});
```

❌ **Mauvais :**

```js
const commentSchema = new mongoose.Schema({
  text: String,
  user: { type: String, ref: "Users" }, // Mauvais : ref incorrecte
  movie: { type: String }               // Mauvais : doit être ObjectId
});
```

📌 **Pourquoi ?**

- La `ref` doit correspondre **exactement** au nom du modèle.
- `ObjectId` est **plus efficace** qu’une chaîne de caractères pour stocker une relation.

------

## 🔹 **5. Définir les timestamps (`createdAt`, `updatedAt`)**

✅ **Toujours activer les timestamps pour suivre la création/modification**

✔ **Bon :**

```js
const userSchema = new mongoose.Schema({
  name: String,
}, { timestamps: true });
```

Mongoose va **ajouter automatiquement** les champs `createdAt` et `updatedAt`.

❌ **Mauvais :**

```js
const userSchema = new mongoose.Schema({
  name: String,
  createdAt: Date, // Mauvais : pas nécessaire avec timestamps
  updatedAt: Date
});
```

------

## 🔹 **6. Valider les données avec des contraintes**

✅ **Toujours définir des `required`, `unique`, et `default` si nécessaire**

✔ **Bon :**

```js
const userSchema = new mongoose.Schema({
  email: { type: String, required: true, unique: true }, // Obligatoire et unique
  password: { type: String, required: true },
  age: { type: Number, min: 18, max: 100 }, // Âge entre 18 et 100
});
```

❌ **Mauvais :**

```js
const userSchema = new mongoose.Schema({
  email: String, // Mauvais : pas de validation
  password: String
});
```

------

## 🔹 **7. Utiliser des Indexes pour optimiser les requêtes**

✅ **Mettre un index sur les champs souvent recherchés**
 ✅ **Utiliser `unique: true` pour éviter les doublons**

✔ **Bon :**

```js
const userSchema = new mongoose.Schema({
  email: { type: String, unique: true, index: true }
});
```

❌ **Mauvais :**

```js
const userSchema = new mongoose.Schema({
  email: { type: String } // Mauvais : pas d’index, recherche lente
});
```

------

## 🔹 **8. Utiliser `lean()` pour les requêtes en lecture**

✅ **Utiliser `.lean()` pour optimiser les performances**
 ✅ **Uniquement pour la lecture (GET), pas pour les mises à jour (POST, PUT, DELETE)**

✔ **Bon :**

```js
const users = await User.find().lean(); // Retourne un objet JS normal
```

❌ **Mauvais :**

```js
const users = await User.find(); // Mauvais : renvoie des objets Mongoose (plus lourd)
```

📌 **Pourquoi ?**

- `.lean()` **améliore la performance** en retournant un objet JS natif au lieu d’un objet Mongoose.

------

## 🔹 **9. Gérer les erreurs avec `try/catch`**

✅ **Toujours entourer les requêtes Mongoose avec un `try/catch`**
 ✅ **Utiliser `next()` dans Express pour gérer les erreurs**

✔ **Bon :**

```js
app.get("/users/:id", async (req, res, next) => {
  try {
    const user = await User.findById(req.params.id);
    if (!user) return res.status(404).json({ message: "User not found" });
    res.json(user);
  } catch (error) {
    next(error); // Envoyer l'erreur au middleware Express
  }
});
```

❌ **Mauvais :**

```js
app.get("/users/:id", async (req, res) => {
  const user = await User.findById(req.params.id); // Mauvais : pas de try/catch
  res.json(user);
});
```

------

## 🔹 **10. Supprimer proprement (`pre` middleware)**

✅ **Utiliser `pre("remove")` pour supprimer des documents liés**

✔ **Bon :**

```js
userSchema.pre("remove", async function (next) {
  await Comment.deleteMany({ user: this._id }); // Supprime les commentaires liés
  next();
});
```

❌ **Mauvais :**

```js
await User.findByIdAndDelete(userId); // Mauvais : ne supprime pas les relations
```

------

## ✅ **Conclusion : Bonnes pratiques à suivre**

1. **Modèles en PascalCase, au singulier** (`User`, `Movie`)
2. **Collections en minuscule et au pluriel** (`users`, `movies`)
3. **Champs en camelCase** (`firstName`, `createdAt`)
4. **Références (`ref`) avec `ObjectId`** et nom exact du modèle
5. **Timestamps (`timestamps: true`) pour suivre les dates**
6. **Validation (`required`, `unique`, `min`, `max`)**
7. **Utiliser des indexes (`unique: true, index: true`)**
8. **Optimiser les requêtes en lecture avec `.lean()`**
9. **Toujours gérer les erreurs (`try/catch`)**
10. **Utiliser des middlewares (`pre("remove")`) pour la suppression en cascade**

Si tu respectes ces conventions, ton code sera **plus propre, plus efficace et plus facile à maintenir** ! 🚀🔥