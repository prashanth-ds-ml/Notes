---

# 🔥 CRUD DRILLS — ROUND 1 (READ → PREDICT → TYPE → VERIFY)

### Setup (do once)

```js
use sample_mflix
```

---

## 🧪 DRILL 1 — `findOne` vs `find` (VERY COMMON)

### Scenario

> Get **one** movie with title `"Inception"`
> Show only `title` and `year`, exclude `_id`

### ⏸️ Predict BEFORE typing

* How many documents?
* Which fields?

### ✍️ Type:

```js
db.movies.findOne(
  { title: "Inception" },
  { title: 1, year: 1, _id: 0 }
)
```

### ✅ Exam takeaway

* `findOne` → single document
* Projection rule respected
* `_id` must be explicitly excluded

---

## 🧪 DRILL 2 — Array equality (classic trap)

### Scenario

> Find movies that belong to **"Drama"** genre

### ⏸️ Predict

* Do we need `$in`?

### ✍️ Type:

```js
db.movies.find({ genres: "Drama" }).limit(3)
```

### ✅ Exam rule

👉 **Direct equality matches array elements**
Most people overthink this in MCQs.

---

## 🧪 DRILL 3 — `$in` operator

### Scenario

> Find movies that are either **Action OR Thriller**

```js
db.movies.find(
  { genres: { $in: ["Action", "Thriller"] } }
).limit(3)
```

### ✅ Exam trap

* `$in` is for **multiple values**
* Direct match is for **single value**

---

## 🧪 DRILL 4 — Projection mistake question

### Scenario

> Identify **incorrect** projection

❌ **WRONG (exam favorite):**

```js
{ title: 1, year: 0 }
```

✅ **Correct options:**

```js
{ title: 1, year: 1 }
{ title: 1, _id: 0 }
```

### 🧠 Rule to remember

* Cannot mix include & exclude
* `_id` is the **only exception**

---

## 🧪 DRILL 5 — `updateOne` with `$set`

### Scenario

> Update movie `"Inception"`
> Set `runtime` to `150`

### ⏸️ Predict

* What changes?
* Will other fields be affected?

### ✍️ Type:

```js
db.movies.updateOne(
  { title: "Inception" },
  { $set: { runtime: 150 } }
)
```

### ✅ Exam takeaway

* `$set` modifies only given fields
* Other fields remain untouched

---

## 🧪 DRILL 6 — FULL document replacement (danger zone)

### Scenario

> Replace the document for `"Inception"` with only title and year

```js
db.movies.replaceOne(
  { title: "Inception" },
  { title: "Inception", year: 2010 }
)
```

### 🚨 Exam trap

* ❌ Everything else is **deleted**
* No `$set` → full overwrite

---

## 🧪 DRILL 7 — `upsert`

### Scenario

> Update movie `"NonExistingMovie"`
> Set year to 2025
> Insert if not found

```js
db.movies.updateOne(
  { title: "NonExistingMovie" },
  { $set: { year: 2025 } },
  { upsert: true }
)
```

### 🧠 Exam logic

* No match → document inserted
* `_id` auto-created

---

## 🧪 DRILL 8 — `updateMany`

### Scenario

> Set `rated: "R"` for all movies before year 1980

```js
db.movies.updateMany(
  { year: { $lt: 1980 } },
  { $set: { rated: "R" } }
)
```

### ✅ Exam takeaway

* Multiple documents updated
* Very common MCQ wording

---

## 🧪 DRILL 9 — `deleteOne` vs `deleteMany`

### Scenario

> Delete **one** movie with title `"NonExistingMovie"`

```js
db.movies.deleteOne({ title: "NonExistingMovie" })
```

### Scenario

> Delete **all** movies before 1900

```js
db.movies.deleteMany({ year: { $lt: 1900 } })
```

---

## 🧪 DRILL 10 — `find().sort().limit()`

### Scenario

> Find **top 5 newest movies**

```js
db.movies.find({})
  .sort({ year: -1 })
  .limit(5)
```

### ⚠️ Exam rule

* `sort()` **before** `limit()`

---


