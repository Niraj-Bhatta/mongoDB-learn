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
 
