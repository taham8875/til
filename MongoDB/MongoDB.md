# MongoDB Cheat Sheet

![MongoDB Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/MongoDB_Logo.svg)






## Introduction

MongoDB is a document database which belongs to a family of databases called NoSQL - not only SQL. Instead of storing data in tables as is done in a "classical" relational database, MongoDB stores structured data as JSON-like documents with dynamic schemas (MongoDB calls the format BSON), making the integration of data in certain types of applications easier and faster.

It is famous for being used with Node.js, but it is also used with other languages such as Python.

## NoSQL vs SQL Analogy

| NoSQL      | SQL      |
| ---------- | -------- |
| Database   | Database |
| Collection | Table    |
| Document   | Row      |
| Field      | Column   |


## Installation

Installation instructions can be found [here](https://www.mongodb.com/try/download/community).

After installation install `mongosh` from [here](https://www.mongodb.com/docs/mongodb-shell/install).

`mongosh` is a shell for MongoDB, which is a program that enables you to interact with the database through the command line.

## Starting the database

To start `mongosh`, run the following command in the terminal:

```bash
$ mongosh
Current Mongosh Log ID: 64916259823ba4574a559ad0
Connecting to:          mongodb://127.0.0.1:27017/?directConnection=true&serverSelectionTimeoutMS=2000&appName=mongosh+1.10.0
Using MongoDB:          6.0.6
Using Mongosh:          1.10.0

For mongosh info see: https://docs.mongodb.com/mongodb-shell/

------
   The server generated these startup warnings when booting
   2023-06-19T22:55:50.785+03:00: Access control is not enabled for the database. Read and write access to data and configuration is unrestricted
------

test>
```

`test` is the name of the database but it does not exist, it will be created automatically if you add data to it.

To view the databases that exist, run the following command:

```bash
test> show dbs
admin   40.00 KiB
config  72.00 KiB
local   40.00 KiB
```

It displayed the databases that exist in the system.

To clean the screen, run the following command:

```bash
test> cls
```

To exit `mongosh`, run the following command:

```bash
test> exit
```

## Creating a database

To create a database, run the following command:

```bash
test> use appdb
switched to db appdb
```

`appdb` is the name of the database.

To view the current database, run the following command:

```bash
appdb> db
appdb
```

## Inserting data

To insert data, run the following command:

```bash
appdb> db.users.insertOne({name: "John", age: 30})
{
  acknowledged: true,
  insertedId: ObjectId("60ce2b0f0b0a9a0a6c7b7b0f")
}
```

`users` is the name of the collection, which is similar to a table in a relational database.

`insertOne` is the command to insert one document.

`{name: "John", age: 30}` is the document to insert.

`ObjectId("60ce2b0f0b0a9a0a6c7b7b0f")` is the id of the document, which is generated automatically.

To see all the documents in the collection, run the following command:

```bash
appdb> db.users.find()
[
  {
    _id: ObjectId("60ce2b0f0b0a9a0a6c7b7b0f"),
    name: 'John',
    age: 30
  }
]
```

Inserted documents doesn't have to have the same structure, for example:

```bash
appdb> db.users.insertOne({name: "Jane", age: 25, address: {city: "New York", country: "USA"}})
{
  acknowledged: true,
  insertedId: ObjectId("60ce2c0a0b0a9a0a6c7b7b10")
}
```

To insert multiple documents, run the following command:

```bash
appdb> db.users.insertMany([{name: "Jack", age: 40}, {name: "Jill", age: 35}])
{
  acknowledged: true,
  insertedIds: {
    '0': ObjectId("649165c8823ba4574a559ad4"),
    '1': ObjectId("649165c8823ba4574a559ad5")
  }
}
```

`insertMany` is the command to insert multiple documents.

`[{name: "Jack", age: 40}, {name: "Jill", age: 35}]` is the documents to insert.

`insertedIds` is the ids of the documents, which are generated automatically.

We will insert dummy data to the database to use it in the examples:

```bash
# insert five users
appdb> db.users.insertMany([{name: "Jane", age: 25, address: {city: "New York", country: "USA"}, hobbies : ["Swimming"]},{name: "Billy", age: 40, address: {city: "London", country: "UK"}, hobbies : ["Reading"]}])
```

`db.users.find()` can be chained with `limit()` to limit the number of documents to display:

```bash
appdb> db.users.find().limit(2)
[
  { _id: ObjectId("64916433823ba4574a559ad1"), name: 'John', age: 30 },
  {
    _id: ObjectId("6491658d823ba4574a559ad2"),
    name: 'Jane',
    age: 25,
    address: { city: 'New York', country: 'USA' }
  }
]
```

To sort the documents, run the following command:

```bash
appdb> db.users.find().sort({name: 1}).limit(2) // alphabetical order
appdb> db.users.find().sort({name: -1}).limit(2) // reverse alphabetical order
```

`1` is for ascending order and `-1` is for descending order.

To sort by multiple fields, run the following command:

```bash
appdb> db.users.find().sort({name: 1, age: -1}).limit(2)
```

`skip()` can be used to skip the first `n` documents:

```bash
appdb> db.users.find().skip(2).limit(2)
```

To find a document by `name`, run the following command:

```bash
appdb> db.users.find({name: "John"})
[
  { _id: ObjectId("64916433823ba4574a559ad1"), name: 'John', age: 30 }
]
```

To find a document by `age`, run the following command:

```bash
appdb> db.users.find({age: 40})
[
  { _id: ObjectId("649165c8823ba4574a559ad4"), name: 'Jack', age: 40 },
  {
    _id: ObjectId("649166e9823ba4574a559ad7"),
    name: 'Billy',
    age: 40,
    address: { city: 'London', country: 'UK' },
    hobbies: [ 'Reading' ]
  }
]
```

To return only the `name` field, run the following command:

```bash
appdb> db.users.find({age: 40}, { name: 1, age:1 })
[
  { _id: ObjectId("649165c8823ba4574a559ad4"), name: 'Jack', age: 40 },
  { _id: ObjectId("649166e9823ba4574a559ad7"), name: 'Billy', age: 40 } ]
```

`1` is for including the field and `0` is for excluding the field.

To return only the `name` field and exclude the `_id` field, run the following command:

```bash
appdb> db.users.find({age: 40}, { name: 1, age:1, _id: 0 })
[
  { name: 'Jack', age: 40 },
  { name: 'Billy', age: 40 }
]
```

To return all the fields except the `name` field, run the following command:

```bash
appdb> db.users.find({age: 40}, { name: 0 })
[
  { _id: ObjectId("649165c8823ba4574a559ad4"), age: 40 },
  {
    _id: ObjectId("649166e9823ba4574a559ad7"),
    age: 40,
    address: { city: 'London', country: 'UK' },
    hobbies: [ 'Reading' ]
  }
]
```

To use more complex queries, for example:

```bash
appdb> db.users.find({ name: {$eq: "Jack"}}) // equal
appdb> db.users.find({ name: {$ne: "Jack"}}) // not equal
appdb> db.users.find({ age: {$gt: 30}}) // greater than
appdb> db.users.find({ age: {$gte: 30}}) // greater than or equal
appdb> db.users.find({ age: {$lt: 30}}) // less than
appdb> db.users.find({ age: {$lte: 30}}) // less than or equal
appdb> db.users.find({ age: {$in: [30, 40]}}) // in
appdb> db.users.find({ age: {$nin: [30, 40]}}) // not in
appdb> db.users.find({ name: {$regex: /^J/}}) // starts with
appdb> db.users.find({ name: {$regex: /n$/}}) // ends with
appdb> db.users.find({ name: {$regex: /.*a.*/}}) // contains
```

To query the document in which the hobbies field exists:

```bash
appdb> db.users.find({ hobbies: {$exists: true}})
[
  {
    _id: ObjectId("6491658d823ba4574a559ad2"),
    name: 'Jane',
    age: 25,
    address: { city: 'New York', country: 'USA' },
    hobbies: [ 'Swimming' ]
  },
  {
    _id: ObjectId("649166e9823ba4574a559ad7"),
    name: 'Billy',
    age: 40,
    address: { city: 'London', country: 'UK' },
    hobbies: [ 'Reading' ]
  }
]
```

To query the document in which the hobbies field does not exist:

```bash
appdb> db.users.find({ hobbies: {$exists: false}})
```

Combine multiple conditions:

```bash
appdb> db.users.find({ age: {$gt: 30, $lte:50}, name: "Jack"})
```

`$and` and `$or` can be used to combine multiple conditions: (they rake an array of conditions)

```bash
appdb> db.users.find({ $and: [{ age: {$gt: 30, $lte:50}}, {name: "Jack"}]})
appdb> db.users.find({ $or: [{ age: {$gt: 30, $lte:50}}, {name: "Jack"}]})
```

`$not` can be used to negate a condition:

```bash
appdb> db.users.find({ age: {$not: {$gt: 30, $lte:50}}})
```

`$nor` can be used to negate multiple conditions:

```bash
appdb> db.users.find({ $nor: [{ age: {$gt: 30, $lte:50}}, {name: "Jack"}]})
```

`$where` can be used to write a JavaScript expression:

```bash
appdb> db.users.find({ $where: "this.age > 30 && this.age <= 50 && this.name == 'Jack'"})
```

To use the `$expr` operator, we first need to add `balance` and `debt` dummy data to the `users` collection:

```bash
appdb> db.users.updateOne({name: "Jack"}, {$set: {balance: 1000, debt: 500}})
appdb> db.users.updateOne({name: "Billy"}, {$set: {balance: 2000, debt: 1000}})
```

To find the users who have a balance greater than their debt:

```bash
appdb> db.users.find({$expr: { $gt: ["$balance", "$debt"]}})
```

Note we used `"$balance"` and `"$debt"` instead of `balance` and `debt` because we need to access the corresponding fields in the document.

To find users from London (accessing the `city` field in the `address` subdocument):

```bash
appdb> db.users.find({"address.city": "London"})
```


## Update Documents

To update a document, run the following command:

```bash
appdb> db.users.updateOne({name: "Jack"}, {$set: {age: 50}})
```

To increment the age by 1:

```bash
appdb> db.users.updateOne({name: "Jack"}, {$inc: {age: 1}})
```

To update the name of a coulmn:

```bash
appdb> db.users.updateOne({name: "Jack"}, {$rename: {name: "firstName"}})
```


To remove a field:

```bash
appdb> db.users.updateOne({firstName: "Jack"}, {$unset: {debt: ""}})
```

To push a new item to the `hobbies` array:

```bash
appdb> db.users.updateOne({name: "Billy"}, {$push: {hobbies: "Tennis"}})
```

To remove an item from the `hobbies` array:

```bash
appdb> db.users.updateOne({name: "Billy"}, {$pull: {hobbies: "Tennis"}})
```
To update multiple documents, run the following command:

```bash
appdb> db.users.updateMany({age: {$gt: 30}}, {$set: {age: 30}})
```

To drop the address field from all documents:

```bash
appdb> db.users.updateMany({}, {$unset: {address: ""}})
```

`replaceOne` can be used to replace a document:

```bash
appdb> db.users.find({name: "Billy"})
[
  {
    _id: ObjectId("649166e9823ba4574a559ad7"),
    name: 'Billy',
    age: 30,
    hobbies: [ 'Reading', 'Tennis' ],
    balance: 2000,
    debt: 1000
  }
]
appdb> db.users.replaceOne({name: "Billy"}, {name: "Billy", age: 50})
appdb> db.users.find({name: "Billy"})
[ { _id: ObjectId("649166e9823ba4574a559ad7"), name: 'Billy', age: 50 } ]
```

## Delete Documents

To delete a document, run the following command:

```bash
appdb> db.users.deleteOne({name: "Billy"})
{ acknowledged: true, deletedCount: 1 }
appdb> db.users.find({name: "Billy"})

appdb>
```

To delete multiple documents, run the following command:

```bash
appdb> db.users.deleteMany({age: {$gte: 30}})
{ acknowledged: true, deletedCount: 3 }
appdb> db.users.find({age: {$gte: 30}})

appdb>
```

To drop the entire collection, run the following command:

```bash
appdb> db.dropDatabase()
{ ok: 1, dropped: 'appdb' }
appdb>
```

