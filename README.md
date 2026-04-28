# mongoDB-learn

# MongoDB Local Setup Guide

> Get MongoDB running on your machine and create your first database collection in four simple steps.

---

## Step 1 — Install MongoDB & Mongosh

Download and install the required tools from the official MongoDB website:

- **MongoDB Community Server** → https://www.mongodb.com/try/download/community
- **MongoDB Shell (mongosh)** → https://www.mongodb.com/try/download/shell

Install both on your system and make sure they are added to your system PATH during installation.

---
## Step 2 — Launch the Shell
 
Open **CMD** or your terminal and type:
 
```bash
mongosh
```
 
This connects to your local MongoDB instance at `mongodb://127.0.0.1:27017` by default.
 
> **Note:** Make sure the MongoDB service (`mongod`) is running before launching mongosh. You can start it from Services on Windows or run `mongod` in a separate terminal.
 
---
 
## Step 3 — Select or Create a Database
 
Switch to your target database using:
 
```js
use database_name
```
 
Replace `database_name` with your desired name. If the database does not exist, MongoDB will **create it automatically** once data is inserted.
 
> **Note:** The database won't appear in `show dbs` until at least one document has been added to it.
 
---
 
## Step 4 — Create a Collection
 
Collections are like tables in SQL. Create one inside your database with:
 
```js
db.createCollection("collection_name")
```
 
Replace `collection_name` with your desired name.
 
On success, MongoDB returns:
 
```json
{ "ok": 1 }
```
 
> **Note:** You can also skip this step — MongoDB auto-creates a collection the first time you insert a document into it.
 
---

## Useful Follow-up Commands
 
| Command | Description |
|---|---|
| `show dbs` | List all databases |
| `show collections` | List collections in the current database |
| `db.collection_name.insertOne({ key: "value" })` | Insert your first document |
| `db.collection_name.find()` | View all documents in a collection |
| `db.collection_name.drop()` | Delete a collection |
| `db.dropDatabase()` | Delete the current database |
 
---
 
## Quick Reference
 
```js
// 1. Connect
mongosh
 
// 2. Use or create a database
use my_database
 
// 3. Create a collection
db.createCollection("my_collection")
 
// 4. Insert a document
db.my_collection.insertOne({ name: "Niraj", age: 21 })
 
// 5. Query documents
db.my_collection.find()
```
 
---

##  Collection Operations
 
### Show All Collections
 
```js
show collections
```
 
### Create a Collection
 
```js
db.createCollection("collection_name")
```
 
### Drop (Delete) a Collection
 
```js
db.collection_name.drop()
```
 
### Rename a Collection
 
```js
db.collection_name.renameCollection("new_collection_name")
```
 
### Copy a Collection (manual method)
 
```js
db.old_collection.find().forEach(doc => db.new_collection.insertOne(doc))
```
 
### Count Documents in a Collection
 
```js
db.collection_name.countDocuments()
```
 
### Get Collection Stats
 
```js
db.collection_name.stats()
```
 
---
 ####  Insert Documents
 
### Insert a Single Document
 
```js
db.collection_name.insertOne({
  name: "Alice",
  age: 25,
  city: "Kathmandu"
})
```
 
### Insert Multiple Documents
 
```js
db.collection_name.insertMany([
  { name: "Bob", age: 30, city: "Pokhara" },
  { name: "Carol", age: 22, city: "Lalitpur" },
  { name: "Dave", age: 28, city: "Bhaktapur" }
])
```
 
### Insert with a Custom `_id`
 
```js
db.collection_name.insertOne({
  _id: "user_001",
  name: "Eve",
  age: 27
})
```
 
> **Note:** If `_id` is not provided, MongoDB automatically generates a unique `ObjectId`.
 
---

# 📘 MongoDB Operators — Combined Reference

---

## 🔵 Comparison Operators

```js
db.users.find({ age: { $eq: 21 } })        // equal
db.users.find({ age: { $ne: 21 } })        // not equal
db.users.find({ age: { $gt: 20 } })        // greater than
db.users.find({ age: { $gte: 21 } })       // greater than or equal
db.users.find({ age: { $lt: 25 } })        // less than
db.users.find({ age: { $lte: 25 } })       // less than or equal
db.users.find({ age: { $in: [20, 21, 22] } })   // match any in array
db.users.find({ age: { $nin: [18, 19] } })      // not in array
```

---

## 🟢 Logical Operators

```js
// AND — all conditions must match
db.users.find({
  $and: [
    { age: { $gt: 20 } },
    { city: "Kathmandu" }
  ]
})

// OR — at least one condition matches
db.users.find({
  $or: [{ age: 21 }, { age: 25 }]
})

// NOT — negates a condition
db.users.find({ age: { $not: { $gt: 25 } } })

// Implicit AND with $ne
db.users.find({
  age: { $gt: 20 },
  city: { $ne: "Pokhara" }
})
```

---

## 🟣 Element Operators

```js
db.users.find({ email: { $exists: true } })   // field exists
db.users.find({ age: { $type: "int" } })      // BSON type check
```

---

## 🟡 Evaluation Operators

```js
db.users.find({ name: { $regex: "^N" } })            // regex match (starts with N)
db.users.find({ $text: { $search: "developer" } })   // full-text search
```

---

## 🟠 Array Operators

```js
db.users.find({ skills: { $all: ["HTML", "CSS"] } })          // must contain all
db.users.find({ skills: { $size: 2 } })                       // array length equals 2
db.users.find({ skills: { $elemMatch: { $eq: "JS" } } })      // element-level match
```

---

## 🟩 Update Operators

```js
// $set — update a field
db.users.updateOne({ name: "Niraj" }, { $set: { age: 22 } })

// $inc — increment a numeric field
db.users.updateOne({ name: "Niraj" }, { $inc: { age: 1 } })

// $unset — remove a field
db.users.updateOne({ name: "Niraj" }, { $unset: { city: "" } })

// $push — add element to array
db.users.updateOne({ name: "Niraj" }, { $push: { skills: "MongoDB" } })

// $pull — remove element from array
db.users.updateOne({ name: "Niraj" }, { $pull: { skills: "CSS" } })
```

---

# 📦 MongoDB Aggregation Pipeline — Complete Reference

> The aggregation pipeline is MongoDB's most powerful feature.  
> It processes documents through a sequence of **stages** — each stage transforms the data and passes it to the next.

---

## 🔁 How It Works

```
Collection → [$match] → [$group] → [$sort] → [$project] → Result
```

Think of it like an assembly line. Data goes in one end, passes through stages, comes out transformed at the other end.

```js
db.collection.aggregate([
  { $stage1: { ... } },
  { $stage2: { ... } },
  { $stage3: { ... } }
])
```

---

## 📋 All Major Stages

| Stage | What it does |
|---|---|
| `$match` | Filter documents (like WHERE in SQL) |
| `$group` | Group and summarize documents |
| `$project` | Include/exclude/reshape fields |
| `$sort` | Order the results |
| `$limit` | Keep only N documents |
| `$skip` | Skip N documents |
| `$unwind` | Flatten an array into separate documents |
| `$lookup` | Join another collection (like SQL JOIN) |
| `$addFields` | Add new computed fields |
| `$count` | Count documents |
| `$out` | Write results to a new collection |

---

## 🔍 $match — Filter Documents

Works exactly like `.find()`. Always put `$match` **first** so fewer documents flow through the pipeline.

```js
db.users.aggregate([
  {
    $match: {
      age: { $gte: 18 },
      city: "Kathmandu"
    }
  }
])
```

**Real life use:** Filter only active users, only paid orders, only posts from this month.

---

## 📊 $group — Summarize Data

Groups documents by a field and applies accumulator operators.

```js
db.users.aggregate([
  {
    $group: {
      _id: "$city",              // group by city
      totalUsers: { $sum: 1 },  // count users per city
      avgAge:     { $avg: "$age" },
      maxAge:     { $max: "$age" },
      minAge:     { $min: "$age" }
    }
  }
])
```

### Accumulator operators inside $group

| Operator | Description |
|---|---|
| `$sum` | Add values (use 1 to count) |
| `$avg` | Average of a field |
| `$max` | Maximum value |
| `$min` | Minimum value |
| `$push` | Collect values into an array |
| `$first` | First value in the group |
| `$last` | Last value in the group |

```js
// Group all orders by userId, collect order totals
db.orders.aggregate([
  {
    $group: {
      _id: "$userId",
      totalSpent:  { $sum: "$amount" },
      orderCount:  { $sum: 1 },
      allAmounts:  { $push: "$amount" }   // [200, 500, 150]
    }
  }
])
```

**Real life use:** Dashboard stats, sales reports, user activity summaries.

---

## 🎯 $project — Shape Your Output

Include, exclude, rename, or compute fields.

```js
db.users.aggregate([
  {
    $project: {
      _id: 0,             // exclude
      name: 1,            // include
      email: 1,           // include
      upperName: { $toUpper: "$name" },       // computed
      ageNextYear: { $add: ["$age", 1] }      // computed
    }
  }
])
```

**Real life use:** Return only the fields the frontend needs. Never send passwords or internal fields to the client.

---

## 🔃 $sort — Order Results

```js
db.users.aggregate([
  { $sort: { age: -1 } }   // descending
])

// Sort by multiple fields
db.orders.aggregate([
  { $sort: { city: 1, totalSpent: -1 } }
])
```

---

## ✂️ $limit and $skip — Pagination

```js
// Get page 2 with 10 items per page
db.users.aggregate([
  { $sort:  { createdAt: -1 } },
  { $skip:  10 },
  { $limit: 10 }
])
```

> ⚠️ Always `$sort` before `$skip` — without sorting, results are unpredictable.

---

## 📂 $unwind — Flatten Arrays

Turns one document with an array into many documents, one per array element.

```js
// Before $unwind:
// { name: "Niraj", skills: ["HTML", "CSS", "JS"] }

db.users.aggregate([
  { $unwind: "$skills" }
])

// After $unwind — 3 separate documents:
// { name: "Niraj", skills: "HTML" }
// { name: "Niraj", skills: "CSS" }
// { name: "Niraj", skills: "JS" }
```

```js
// Real use: count how many users have each skill
db.users.aggregate([
  { $unwind: "$skills" },
  {
    $group: {
      _id: "$skills",
      count: { $sum: 1 }
    }
  },
  { $sort: { count: -1 } }
])
// Result: [{ _id: "JS", count: 45 }, { _id: "HTML", count: 38 }, ...]
```

---

## 🔗 $lookup — Join Two Collections

Like a SQL LEFT JOIN. Pulls in documents from another collection.

```js
// orders collection has: { userId, amount, product }
// users collection has: { _id, name, email }

db.orders.aggregate([
  {
    $lookup: {
      from:         "users",     // collection to join
      localField:   "userId",    // field in orders
      foreignField: "_id",       // field in users
      as:           "userInfo"   // output array field name
    }
  },
  { $unwind: "$userInfo" },      // flatten the array (it's 1 user per order)
  {
    $project: {
      amount: 1,
      product: 1,
      "userInfo.name": 1,
      "userInfo.email": 1
    }
  }
])
```

**Real life use:** Show orders with customer details, posts with author info, products with category names.

---

## ➕ $addFields — Add Computed Fields

Like `$project` but keeps all existing fields and just adds new ones.

```js
db.users.aggregate([
  {
    $addFields: {
      skillCount: { $size: "$skills" },
      isAdult:    { $gte: ["$age", 18] },
      fullLabel:  { $concat: ["$name", " from ", "$city"] }
    }
  }
])
```

---

## 🔢 $count — Count Documents

```js
db.users.aggregate([
  { $match: { city: "Kathmandu" } },
  { $count: "totalInKathmandu" }
])
// Result: { totalInKathmandu: 142 }
```

---

## 💾 $out — Save Results to a Collection

Writes the final pipeline output into a new (or existing) collection.

```js
db.orders.aggregate([
  { $group: { _id: "$userId", totalSpent: { $sum: "$amount" } } },
  { $out: "user_spending_summary" }  // creates/replaces this collection
])
```

> ⚠️ `$out` replaces the entire collection. Use carefully in production.

---

## 🏗️ Real World Examples

### Example 1 — Sales dashboard

```js
// Total revenue, order count, and avg order value per city
db.orders.aggregate([
  { $match: { status: "completed" } },
  {
    $group: {
      _id:        "$city",
      revenue:    { $sum: "$amount" },
      orderCount: { $sum: 1 },
      avgOrder:   { $avg: "$amount" }
    }
  },
  { $sort: { revenue: -1 } },
  { $limit: 10 }
])
```

### Example 2 — Top skills across all users

```js
db.users.aggregate([
  { $unwind: "$skills" },
  { $group: { _id: "$skills", count: { $sum: 1 } } },
  { $sort:  { count: -1 } },
  { $limit: 5 }
])
```

### Example 3 — Orders with user details (JOIN)

```js
db.orders.aggregate([
  { $match: { amount: { $gte: 1000 } } },
  {
    $lookup: {
      from: "users", localField: "userId",
      foreignField: "_id", as: "user"
    }
  },
  { $unwind: "$user" },
  {
    $project: {
      amount: 1, product: 1,
      userName:  "$user.name",
      userEmail: "$user.email"
    }
  },
  { $sort: { amount: -1 } }
])
```

### Example 4 — Monthly signups count

```js
db.users.aggregate([
  {
    $group: {
      _id: {
        year:  { $year:  "$createdAt" },
        month: { $month: "$createdAt" }
      },
      newUsers: { $sum: 1 }
    }
  },
  { $sort: { "_id.year": -1, "_id.month": -1 } }
])
```

---

## ⚡ Performance Tips

**1. Always `$match` early**
Put `$match` as the first stage. This reduces the number of documents flowing through the rest of the pipeline.

```js
// ✅ Good — filter first
[{ $match: { status: "active" } }, { $group: ... }]

// ❌ Bad — processes ALL documents before filtering
[{ $group: ... }, { $match: { status: "active" } }]
```

**2. Index your `$match` fields**
If `$match` uses a field with an index, MongoDB uses it — same as `.find()`.

**3. Use `$project` early to reduce document size**
Drop heavy fields you don't need before expensive stages like `$lookup`.

**4. `$limit` before `$sort` when possible**
If you only need the top 5, limit before sorting a huge dataset.

---

## 🆚 Aggregation vs find()

| | `.find()` | `.aggregate()` |
|---|---|---|
| Filter | ✅ | ✅ |
| Projection | ✅ | ✅ |
| Sort / Limit | ✅ | ✅ |
| Group / Count | ❌ | ✅ |
| Join collections | ❌ | ✅ (`$lookup`) |
| Compute new fields | ❌ | ✅ |
| Complex transformations | ❌ | ✅ |

> Use `.find()` for simple reads. Use `.aggregate()` when you need to transform, summarize, or join data.

---

## 🔗 In Mongoose

```js
const result = await User.aggregate([
  { $match: { age: { $gte: 18 } } },
  { $group: { _id: "$city", count: { $sum: 1 } } },
  { $sort: { count: -1 } }
])
```

> ⚠️ Mongoose does **not** apply middleware or schema validation inside `.aggregate()`.  
> It sends the pipeline directly to MongoDB. The result is plain JS objects, not Mongoose documents.

---

*Made with MongoDB Community Edition · mongosh shell*

*Made with MongoDB Community Edition · mongosh shell*
