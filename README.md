# 📚 MongoDB Student Database Project

A practical MongoDB project demonstrating student record management using the MongoDB Shell (`mongosh`). The project covers CRUD operations, comparison and logical query operators, database verification and searching, with the exact command and output documented for every step in the project report.

---

## 📌 Project Overview

This project builds and queries a `students` collection holding academic details of **50 students** across the **AIML, Robotics, FinTech and Biotech** departments. It demonstrates insertion, updation, deletion and querying, the standard operations that form the backbone of any real-world database-driven application.

---

## 🗄️ Database Details

| Property | Value |
|---|---|
| Database | `studentProjectDB` |
| Collection | `students` |
| Records inserted | 50 |
| Records after all operations | 46 |
| Roll Numbers | 401–450 |
| Departments | AIML, Robotics, FinTech, Biotech |
| Database Type | NoSQL (document-oriented) |
| Tool | MongoDB Shell (`mongosh`) |

> **Note on counts:** the roll-number range 401–450 is 50 numbers. Roll 401 is inserted with `insertOne()` and 402–450 with `insertMany()`, giving 50 records. After `deleteOne()` (roll 450) and `deleteMany()` (3 records with marks below 40), **46 records remain**.

---

## 🛠️ Technologies Used

- MongoDB
- MongoDB Shell (`mongosh`)
- MongoDB Query Language
- NoSQL Database

---

## 📊 Student Record Structure

Each student document contains:

- `rollNo`
- `name`
- `age`
- `department`
- `marks`

The `updateMany()` operation also adds a `status` field.

### Example Document

```js
{
  rollNo: 401,
  name: "Advait",
  age: 20,
  department: "AIML",
  marks: 74
}
```

---

## 🔄 CRUD Operations

### 1. Create / Select Database

```js
use studentProjectDB
```

### 2. Insert One

Inserts a single student record (roll number 401).

```js
db.students.insertOne({
  rollNo: 401,
  name: "Advait",
  age: 20,
  department: "AIML",
  marks: 74
})
```

### 3. Insert Many

Inserts 49 student records, roll numbers 402 to 450, in one command.

```js
db.students.insertMany([
  { rollNo: 402, name: "Samaira", age: 21, department: "Robotics", marks: 76 },
  { rollNo: 403, name: "Rehaan", age: 20, department: "FinTech", marks: 69 }
  // ... up to rollNo: 450 (full list in the project report)
])
```

**Departments used:** `AIML`, `Robotics`, `FinTech`, `Biotech`

### 4. Update One

Updates the marks of roll number 401: **74 → 81**.

```js
db.students.updateOne(
  { rollNo: 401 },
  { $set: { marks: 81 } }
)
```

Output: `matchedCount: 1`, `modifiedCount: 1`

### 5. Update Many

Adds `status: "Active"` to every student in the **Robotics** department.

```js
db.students.updateMany(
  { department: "Robotics" },
  { $set: { status: "Active" } }
)
```

Output: `matchedCount: 12`, `modifiedCount: 12`

### 6. Delete One

Deletes the student with roll number 450.

```js
db.students.deleteOne({ rollNo: 450 })
```

Output: `deletedCount: 1`

### 7. Delete Many

Deletes students whose marks are less than 40. Three records match: roll 417 (35 marks), roll 423 (31 marks) and roll 438 (38 marks).

```js
db.students.deleteMany({ marks: { $lt: 40 } })
```

Output: `deletedCount: 3`

---

## 🔎 Query Operations

| Operator | Meaning | Query | Results |
|---|---|---|---|
| `$gt` | Greater than | `{ marks: { $gt: 80 } }` | 21 |
| `$lt` | Less than | `{ marks: { $lt: 80 } }` | 24 |
| `$eq` | Equal to | `{ marks: { $eq: 85 } }` | 3 |
| `$gte` | Greater than or equal to | `{ marks: { $gte: 80 } }` | 22 |
| `$ne` | Not equal to | `{ marks: { $ne: 85 } }` | 43 |

(Result counts are for the 46 documents left after all update and delete operations.)

### `$gt`: Greater Than

```js
db.students.find({ marks: { $gt: 80 } })
```

### `$lt`: Less Than

```js
db.students.find({ marks: { $lt: 80 } })
```

### `$eq`: Equal To

Find students with exactly 85 marks.

```js
db.students.find({ marks: { $eq: 85 } })
```

### `$gte`: Greater Than or Equal To

```js
db.students.find({ marks: { $gte: 80 } })
```

### `$ne`: Not Equal To

```js
db.students.find({ marks: { $ne: 85 } })
```

---

## 🧠 Logical Operators

### `$and`

Find students whose marks are greater than 70 **and** age is less than 21. (23 results)

```js
db.students.find({
  $and: [
    { marks: { $gt: 70 } },
    { age: { $lt: 21 } }
  ]
})
```

### `$or`

Find students from either AIML or FinTech. (23 results)

```js
db.students.find({
  $or: [
    { department: "AIML" },
    { department: "FinTech" }
  ]
})
```

### `$not`

Find students whose marks are not greater than 80. (25 results)

```js
db.students.find({ marks: { $not: { $gt: 80 } } })
```

---

## 📋 Verification Operations

Show current database

```js
db
```

Show collections

```js
show collections
```

Display all students

```js
db.students.find()
```

Display records in readable format

```js
db.students.find().pretty()
```

Verify the updated record (marks should be 81)

```js
db.students.findOne({ rollNo: 401 })
```

Confirm the deleted records are gone (returns nothing)

```js
db.students.find({ rollNo: { $in: [417, 423, 438, 450] } })
```

Count total documents (expected: **46**)

```js
db.students.countDocuments()
```

Count students per department (expected values after all operations)

```js
db.students.countDocuments({ department: "AIML" })      // 13
db.students.countDocuments({ department: "Robotics" })  // 11
db.students.countDocuments({ department: "FinTech" })   // 10
db.students.countDocuments({ department: "Biotech" })   // 12
```

---

## 🔍 Student Search Operations

Find student by roll number

```js
db.students.find({ rollNo: 401 })
```

Find students by department

```js
db.students.find({ department: "AIML" })
db.students.find({ department: "Robotics" })
db.students.find({ department: "FinTech" })
db.students.find({ department: "Biotech" })
```

---

## 🎯 Learning Objectives

This project provides practical understanding of:

- NoSQL databases
- MongoDB databases
- Collections and documents
- CRUD operations
- MongoDB Shell
- MongoDB Query Language
- Comparison operators
- Logical operators
- Updating documents
- Deleting documents
- Searching and filtering records
- Counting documents
- Database verification

---

## 🚀 How to Run

**Step 1: Install MongoDB**

Install MongoDB and MongoDB Shell (`mongosh`).

**Step 2: Open MongoDB Shell**

```bash
mongosh
```

**Step 3: Select Database**

```js
use studentProjectDB
```

**Step 4: Run the commands**

Run the commands from the project in sequence (insert, update, delete, then queries) and verify the output after each operation. The `students` collection is created automatically on the first insert.

---

## 📂 Project Structure

```text
MongoDB-Student-Database-Operations/
│
├── README.md
│
└── MongoDB_Student_Database_Report_v4.pdf
```

---

## 🎓 Academic Information

| Field | Details |
|---|---|
| Project | MongoDB Student Database Operations |
| Course | Database Management System (NoSQL / MongoDB), Project Based Experimental Learning |
| Program | BCA Data Science (Section BCADS27), 3rd Semester |
| University | Babu Banarasi Das University, Lucknow, U.P |
| Submitted to | Mr. Harendra Singh Rajpoot |

---

## 👨‍💻 Author

**Taha Qadri**

BCA Data Science, Babu Banarasi Das University

---

## ⭐ Project Highlights

- ✅ MongoDB NoSQL Database
- ✅ Student Records Management
- ✅ CRUD Operations
- ✅ Insert One & Insert Many
- ✅ Update One & Update Many
- ✅ Delete One & Delete Many
- ✅ Comparison Operators (`$gt`, `$lt`, `$eq`, `$gte`, `$ne`)
- ✅ Logical Operators (`$and`, `$or`, `$not`)
- ✅ Department-wise Searching
- ✅ Document Counting
- ✅ Database Verification
- ✅ MongoDB Shell Practice
- ✅ Academic Practical Project

---

## 📜 License

This project is created for educational and academic purposes.

You are welcome to study and use the concepts demonstrated in this project for learning and practical practice.
