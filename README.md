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

// 📘 MongoDB Operators - Combined Reference

// =======================
// 🔹 Comparison Operators
// =======================
db.users.find({ age: { $eq: 21 } })   // equal
db.users.find({ age: { $ne: 21 } })   // not equal
db.users.find({ age: { $gt: 20 } })   // greater than
db.users.find({ age: { $gte: 21 } })  // greater than or equal
db.users.find({ age: { $lt: 25 } })   // less than
db.users.find({ age: { $lte: 25 } })  // less than or equal
db.users.find({ age: { $in: [20, 21, 22] } })     // match any
db.users.find({ age: { $nin: [18, 19] } })        // not in

// =======================


// 🔹 Logical Operators
// =======================
db.users.find({
  $and: [
    { age: { $gt: 20 } },
    { city: "Kathmandu" }
  ]
})

db.users.find({
  $or: [
    { age: 21 },
    { age: 25 }
  ]
})

db.users.find({
  age: { $not: { $gt: 25 } }
})

db.users.find({
  age: { $gt: 20 },
  city: { $ne: "Pokhara" }
})

// =======================
// 🔹 Element Operators
// =======================
db.users.find({ email: { $exists: true } })   // field exists
db.users.find({ age: { $type: "int" } })      // type check

// =======================
// 🔹 Evaluation Operators
// =======================
db.users.find({ name: { $regex: "^N" } })     // starts with N
db.users.find({ $text: { $search: "developer" } }) // text search



// 🔹 Array Operators
// =======================
db.users.find({ skills: { $all: ["HTML", "CSS"] } })  // must have all
db.users.find({ skills: { $size: 2 } })               // array size
db.users.find({ skills: { $elemMatch: { $eq: "JS" } } }) // match element

// =======================
// 🔹 Update Operators
// =======================
db.users.updateOne(
  { name: "Niraj" },
  { $set: { age: 22 } }
)

db.users.updateOne(
  { name: "Niraj" },
  { $inc: { age: 1 } }
)

db.users.updateOne(
  { name: "Niraj" },
  { $unset: { city: "" } }
)

db.users.updateOne(
  { name: "Niraj" },
  { $push: { skills: "MongoDB" } }
)

db.users.updateOne(
  { name: "Niraj" },
  { $pull: { skills: "CSS" } }
)

// =======================
// 🔹 Aggregation Operators
// =======================
db.users.aggregate([
  {
    $group: {
      _id: "$city",
      totalUsers: { $sum: 1 },
      avgAge: { $avg: "$age" },
      maxAge: { $max: "$age" },
      minAge: { $min: "$age" }
    }
  }
])

// =======================
// 🔹 Projection Operators
// =======================
db.users.find(
  {},
  {
    name: 1,
    age: 1,
    _id: 0
  }
)

// =======================
// 🔹 Sorting & Limiting
// =======================
db.users.find().sort({ age: 1 })   // ascending
db.users.find().sort({ age: -1 })  // descending
db.users.find().limit(5)

// =======================
// 🔹 Index Operator
// =======================
db.users.createIndex({ name: 1 })


*Made with MongoDB Community Edition · mongosh shell*
 
