# Mongoose Cheat Sheet

![Mongoose Logo](assets/mongooses-logo.png)

Mongoose is a MongoDB object modeling tool. It is used to translate between objects in code and the representation of those objects in MongoDB.

## Installation

```bash
npm install mongoose
```

## Connecting to MongoDB

```javascript
const mongoose = require('mongoose');

mongoose.connect('mongodb://127.0.0.1:27017/test');
```

## Defining a Schema

Schemas are used to define the structure of documents in MongoDB. They are used both for validating documents and for translating between objects in code and the representation of those objects in MongoDB.

Let's define a schema for a user object in `User.js`:

```javascript
const mongoose = require('mongoose');
const userSchema = new mongoose.Schema({
  name: String,
  email: String,
  age: Number
});
```

Schema takes a key value pair, where the key is the name of the property in the MongoDB document and the value is the type of data in that property.

## Creating a Model

Models are fancy constructors compiled from Schema definitions. An instance of a model is called a document. Models are responsible for creating and reading documents from the underlying MongoDB database.

Let's create a model for our user schema:

```javascript
module.exports = mongoose.model('User', userSchema);
```

Now let's import our model and create a new user:

```javascript
const mongoose = require("mongoose");
const User = require("./User");

mongoose.connect("mongodb://127.0.0.1:27017/test");

const run = async () => {
  const user = new User({
    name: "John Doe",
    email: "john@example.com",
    age: 25,
  });

  await user.save();
  console.log(user);
};

run();
```

Another way to create a new user is to use the `create` method:

```javascript
const user = await User.create({
  name: "John Doe",
  email: "john@example.com",
  age: 25,
});
```

## Schema Types

Besides `String` and `Number`, Mongoose supports many other types:

```javascript
const userSchema = new mongoose.Schema({
  name: String,
  email: String,
  age: Number,
  createdAt: Date,
  updatedAt: Date,
  isVerified: Boolean,
  bestFriend: mongoose.Schema.Types.ObjectId,
  hobbies: [String],
  address: {
    street: String,
    city: String,
  },
});
```

The nested schema above can get a little complicated. Let's break it down:

```javascript
const addressSchema = new mongoose.Schema({
  street: String,
  city: String,
});

const userSchema = new mongoose.Schema({
  name: String,
  email: String,
  age: Number,
  createdAt: Date,
  updatedAt: Date,
  isVerified: Boolean,
  bestFriend: {
    type: mongoose.Schema.Types.ObjectId
    ref: 'User'
  },
  hobbies: [String],
  address: addressSchema,
});
```

another validations can be added to the schema:

```javascript
const userSchema = new mongoose.Schema({
  name: String,
  email: {
    type: String,
    minlength: 6,
    required: true,
    unique: true,
    lowercase: true,
  },
    age: {
      type: Number,
      min: 1,
      max: 100,
      validate :{
        validator: value => value % 2 === 0;
        message: props => `${props.value} is not an even number!`
      },
    },
  createdAt: {
    type: Date,
    default: () => new Date(),
    immutable: true, // prevents updates
  },
})
```

## Finding Documents

Find all documents

```javascript
const users = await User.find();
```

Find documents by criteria

```javascript
const users = await User.find({ name: "John Doe" });
```

Delete documents by criteria

```javascript
const users = await User.deleteMany({ name: "John Doe" });
```

If you understand mongoDB query language, you can use it with mongoose, but it can be a little bit confusing, so mongoose provides a helper methods to make it easier:

```javascript
const users = await User.find().where("name").equals("John Doe");
```

chain multiple where clauses:

```javascript
const users = await User.find()
  .where("name")
  .equals("John Doe")
  .where("age")
  .gt(18)
  .lt(60);
  .limit(10)
  .select("name")
```

