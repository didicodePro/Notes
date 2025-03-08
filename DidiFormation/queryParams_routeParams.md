Yes exactement ! 😃🔥 **Il y a deux types de paramètres en Express :**

------

## **📌 1️⃣ Route Parameters (`:param`)**

➡️ **Les "route params" font partie de l’URL** et sont souvent utilisés pour **identifier une ressource unique** (ex: un film, un utilisateur…).

### **Exemple :**

```
GET /movies/123
```

Ici, `123` est un **id de film**.

📌 **Comment récupérer `id` en Express ?**

```js
app.get("/movies/:id", (req, res) => {
  console.log(req.params.id); // "123" si l'URL est /movies/123
  res.send(`Film ID: ${req.params.id}`);
});
```

🟢 **Résultat :**

```
Film ID: 123
```

------

## **📌 2️⃣ Query Parameters (`?key=value`)**

➡️ **Les "query params" sont après `?`** et servent pour **les filtres, la pagination, le tri...**

### **Exemple :**

```
GET /movies?limit=5&sort=desc
```

📌 **Comment récupérer `limit` et `sort` en Express ?**

```javascript
app.get("/movies", (req, res) => {
  console.log(req.query); // { limit: '5', sort: 'desc' }
  res.send(`Limit: ${req.query.limit}, Sort: ${req.query.sort}`);
});
```

🟢 **Résultat :**

```
Limit: 5, Sort: desc
```

------

## **💡 Différence entre Route Params et Query Params**

| **Type**         | **Exemple d'URL**             | **Utilisation**                        | **Comment le récupérer ?**          |
| ---------------- | ----------------------------- | -------------------------------------- | ----------------------------------- |
| **Route Params** | `/movies/:id` → `/movies/123` | Identifier une ressource (ex: un film) | `req.params.id`                     |
| **Query Params** | `/movies?limit=5&sort=desc`   | Filtrer, paginer, trier...             | `req.query.limit`, `req.query.sort` |

------

## **🎯 Conclusion**

✔️ **`req.params`** = Pour récupérer **des valeurs dynamiques dans l’URL** (`/movies/:id`).
 ✔️ **`req.query`** = Pour récupérer **des options dans l’URL** (`?limit=10&sort=desc`).
 ✔️ **Les deux peuvent être combinés** ! (ex: `/movies/123/comments?limit=5`).

🚀 **Bref, maintenant tu es un pro des routes Express !** 😎🔥

Non ! 🚀 **Les query parameters (`req.query`) ne sont pas prédéfinis**, tu peux utiliser **n'importe quel nom** selon tes besoins.

------

## **📌 1️⃣ Les query params sont libres**

➡️ Tu peux utiliser **les noms que tu veux**, ils sont juste des **paires clé-valeur**.
 Exemples :

```
GET /movies?limit=10&sort=asc&genre=action&page=2
```

📌 **Comment récupérer ces paramètres dans Express ?**

```js
app.get("/movies", (req, res) => {
  console.log(req.query);
  res.json(req.query);
});
```

🔹 **Si l’URL est :** `/movies?limit=10&sort=asc&genre=action&page=2`
 🔹 **Express renvoie :**

```json
{
  "limit": "10",
  "sort": "asc",
  "genre": "action",
  "page": "2"
}
```

➡️ **Tous ces paramètres sont personnalisables**, il n'y a **pas de liste fixe**.

------

## **📌 2️⃣ Quelques query params "classiques" utilisés en API**

Même si tu peux mettre **ce que tu veux**, certaines conventions sont souvent utilisées :

| **Paramètre** | **Utilisation**                                              |
| ------------- | ------------------------------------------------------------ |
| `limit`       | Nombre de résultats à retourner                              |
| `page`        | Numéro de la page pour la pagination                         |
| `sort`        | Tri des résultats (`asc` ou `desc`)                          |
| `orderBy`     | Champ sur lequel trier (`name`, `date`, etc.)                |
| `filter`      | Appliquer un filtre (`?filter=action` pour les films d’action) |
| `search`      | Rechercher un élément (`?search=batman`)                     |
| `fields`      | Sélectionner certains champs (`?fields=title,year`)          |

------

## **📌 3️⃣ Exemples concrets d'utilisation**

#### 🔹 **1️⃣ Pagination**

```
GET /movies?page=2&limit=5
```

🔹 Renvoie **5 films**, en commençant par la **page 2**.

#### 🔹 **2️⃣ Tri**

```
GET /movies?sort=desc&orderBy=year
```

🔹 Trie les films **par année (`year`) en ordre décroissant**.

#### 🔹 **3️⃣ Recherche**

```
GET /movies?search=inception
```

🔹 Filtre les films **qui contiennent "Inception"**.

#### 🔹 **4️⃣ Sélectionner certains champs**

```
GET /movies?fields=title,year
```

🔹 Renvoie **uniquement** `title` et `year` pour chaque film.

------

## **🎯 Conclusion**

✔️ **Pas de query params prédéfinis**, tu peux mettre ce que tu veux !
 ✔️ Il existe des **conventions courantes** (`limit`, `sort`, `page`, `search`, etc.).
 ✔️ Les query params sont **très flexibles** et permettent de **filtrer, paginer et trier** facilement tes données.

🚀 **Bref, tu peux créer tes propres paramètres selon les besoins de ton API !** 😎🔥

