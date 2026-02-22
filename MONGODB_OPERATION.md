# MongoDB CRUD Cheat Sheet

A one-page reference comparing **Native MongoDB Driver** and **Mongoose Model** CRUD operations.

---

## CREATE

| Action        | Native Mongo Driver | Mongoose                 |
| ------------- | ------------------- | ------------------------ |
| Insert one    | `insertOne(doc)`    | `Model.create(doc)`      |
| Insert many   | `insertMany(docs)`  | `Model.insertMany(docs)` |
| Save instance | ❌                   | `new Model(doc).save()`  |

```js
// Native
db.users.insertOne({ name: "John" });

// Mongoose
User.create({ name: "John" });
```

---

## READ

| Action        | Native                  | Mongoose               |
| ------------- | ----------------------- | ---------------------- |
| Find many     | `find(query).toArray()` | `Model.find(query)`    |
| Find one      | `findOne(query)`        | `Model.findOne(query)` |
| By ID         | `findOne({_id})`        | `Model.findById(id)`   |
| Select fields | projection              | `.select()`            |
| Sort          | `.sort()`               | `.sort()`              |
| Limit         | `.limit()`              | `.limit()`             |

```js
// Native
db.users.find({ age: 20 }).limit(5).toArray();

// Mongoose
User.find({ age: 20 }).limit(5);
```

---

## UPDATE

| Action        | Native               | Mongoose              |
| ------------- | -------------------- | --------------------- |
| Update one    | `updateOne()`        | `updateOne()`         |
| Update many   | `updateMany()`       | `updateMany()`        |
| Find + update | `findOneAndUpdate()` | `findOneAndUpdate()`  |
| By ID update  | ❌                    | `findByIdAndUpdate()` |
| Replace doc   | `replaceOne()`       | `replaceOne()`        |

```js
// Native
db.users.updateOne(
  { _id },
  { $set: { age: 30 } }
);

// Mongoose
User.findByIdAndUpdate(id, { age: 30 }, { new: true });
```

---

## DELETE

| Action        | Native               | Mongoose              |
| ------------- | -------------------- | --------------------- |
| Delete one    | `deleteOne()`        | `deleteOne()`         |
| Delete many   | `deleteMany()`       | `deleteMany()`        |
| Find + delete | `findOneAndDelete()` | `findOneAndDelete()`  |
| By ID delete  | ❌                    | `findByIdAndDelete()` |

---

## UTILITIES (Very Common)

| Use case      | Native             | Mongoose           |
| ------------- | ------------------ | ------------------ |
| Count         | `countDocuments()` | `countDocuments()` |
| Exists        | `findOne()`        | `exists()`         |
| Aggregate     | `aggregate()`      | `aggregate()`      |
| Transactions  | Sessions           | Sessions           |
| Populate refs | ❌                  | `populate()`       |

---

## Mental Model 🧠

> **Mongoose method = Native driver + extra sugar**
> (validation, middleware, casting)

---

## When NOT to Use Mongoose 🚫

### ❌ Avoid Mongoose if:

1. **Maximum performance is required**

   * Native driver is faster for analytics, logs, bulk writes

2. **Your schema is highly dynamic**

   ```js
   { key: "anything", value: any }
   ```

3. **You are writing DB utilities or migrations**

   * Middleware can cause unexpected side effects

4. **You want full visibility into MongoDB behavior**

---

## When Mongoose Shines ✅

* Schema validation
* Cleaner models
* `populate()` (joins)
* Middleware (`pre`, `post` hooks)
* REST / MVC style APIs

---

## Pro Dev Pattern 💡

```txt
API → Mongoose (business logic)
Jobs / Scripts → Native Mongo Driver
```

Best of both worlds.

---

Happy building 🚀
