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
 
*Made with MongoDB Community Edition · mongosh shell*
 
