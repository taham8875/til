# Protocol Buffers

Protocol buffers are mechanism used to serialize structured data. It is language neutral, platform neutral, extensible and efficient. It is used to serialize structured data for efficient processing and/or storage.

Protocol buffers are developed at Google and where made open source in 2008.

Protocol buffers comes with a code generation tool, which can be used to generate data access classes in different languages. The code generation tool is called `protoc` and it is used to compile the schema definition to generate data access classes. This is why we call protobufs language neutral, because we can use the same schema definition to generate data access classes in different languages.

## Why Protocol Buffers?

- Language neutral (means that you can use it with any programming language)
- Less space (smaller in size than JSON), so less bytes are transmitted over the network or stored in the disk
- Schema (data structure) is defined in a `.proto` file, which can be used to generate data access classes and prevent invalid data from being used
- Language independent data types, so you can use the same schema definition to generate data access classes in different languages (if your backend in go and front end in typescript, you can share types!)

## The problem with JSON

- JSON is a text format, which is inefficient to parse and generate
- JSON is verbose, there are many repeated keys for example and syntax is not very concise

for example, think about how much we wasted here to repeat the keys over and over again (which of course a waste of space and bandwidth):

```json
{
    [
        {
        "id": 123,
        "name": "John",
        "email": "john@gmail.com",
        "phone": "1234567890"
        },
        {
        "id": 456,
        "name": "Jane",
        "email": "jane@gmail.com",
        "phone": "0987654321"
        }
    ]
}
```

- JSON doesn't have a schema, so it is hard to validate data, I can break the schema and add a new field which may introduce bugs in the code

For example: think about this inconsistent json:

```json
{
    [
        {
        "id": 123,
        "name": "John",
        "email": "john@gmail.com",
        "phoneNumber": "1234567890"
        },
        {
        "id": 456,
        "username": "Jane",
        "email": "jane@gmail.com",
        "phone": "0987654321"
        }
    ]
}
```

## Starting with Protocol Buffers

Protocol buffers are defined in `.proto` files, which contains the schema definition. The schema definition is used to generate data access classes in different languages.

Let's start with a simple example, we will define a schema for a `Person` object, which has the following fields:

- `id` (integer)
- `name` (string)
- `email` (string)
- `phone` (string)
- `friends` (list of `Person` objects)

Then we define a schema for `Persons` object, which is a list of `Person` objects.

The schema definition for this object is as follows:

```proto
syntax = "proto3";

message Person {
    int32 id = 1;
    string name = 2;
    string email = 3;
    string phone = 4;
    repeated Person friends = 5;
}

message Persons {
    repeated Person persons = 1;
}
```

Let's break down the schema definition:

- `syntax = "proto3";` is the version of the protocol buffers, we are using version 3
- `message` is a keyword used to define a schema for an object
- `Person` is the name of the object
- `int32 id = 1;` is a field definition, it is an integer field with name `id` and id `1`, tags are used to identify fields in the binary data, so the tag `1` is used to identify the `id` field, and `2` is used to identify the `name` field and so on
- There are may type can be used in the schema definition, for example `int32`, `string`, `bool`, `float`, `double` and `bytes`.
- `repeated` is used to define a list of objects, for example `repeated Person friends = 5;` means that the `friends` field is a list of `Person` objects

So no field names! only tags are used to identify fields in the binary data, this is one of the reasons protobufs are more efficient than JSON, because we don't need to repeat the field names over and over again.

## Compiling the schema

After defining the schema, we need to compile it to generate data access classes in different languages. We can use the `protoc` compiler to compile the schema.

To compile the schema, we need to install the `protoc` compiler, you can download it from [here](https://github.com/protocolbuffers/protobuf/releases/tag/v25.2)

> For the ease of use via the command line, it is recommended to add the `protoc` compiler to the `PATH` environment variable

Then install The protobuf runtime library:

```bash
npm install google-protobuf
```

Then we can compile the schema using the following command:

```bash
protoc --js_out=import_style=commonjs,binary:. person.proto
```

This will generate a `person_pb.js` file, which contains the data access classes for the `Person` object.

## Using the generated classes

Now we can use the generated classes to create a `Person` object and serialize it to binary data:

```javascript
const Person = require('./person_pb');

const John = new Person();
person.setId(123);
person.setName('John');
person.setEmail('john@gmail.com');
person.setPhone('1234567890');

const Jane = new Person();
person.setId(456);
person.setName('Jane');
person.setEmail('jane@gmail.com');

John.addFriends(Jane);

// You can take a look at the proto variables to see the data
console.log(John.serializeBinary()); // outputs binary data
console.log(John.toString()); // outputs readable string
```

Notice how `protoc` output file automatically generated getters and setters for the fields.

## Serializing and deserializing data

Now we can serialize the data to binary data and deserialize it back:

```javascript
const binaryData = John.serializeBinary();

// Now we can send the binary data over the network or store it in the disk


// const binaryData = ... // read the binary data from the network or disk
const person = Person.deserializeBinary(binaryData);
```

## Field tags and schema evolution

- If a field value does not exists, it simply omitted from the encoded record
- You can change the field name in the schema, but you can't change the field tag, because it is used to identify the field in the binary data (if you did, all existing data will be invalid)
- You can add new field easily to the schema, because it will be omitted from the encoded record if it does not exists, this maintain forwards compatibility, old code can read data written by new code
- As long as each field has its own unique tag, backwards compatibility is maintained, new code can read data written by old code, but not that you cannot make it required, because checks will fail on reading old data, to maintain backward compatibility, every field you add after the initial deployment of the schema must be optional or have a default value

## Data type changes and schema evolution

- You can change the data type of a field, but this may lead to losing precision or truncation of data

## Further reading

- [LinkedIn Adopts Protocol Buffers for Microservices Integration and Reduces Latency by up to 60%](https://www.infoq.com/news/2023/07/linkedin-protocol-buffers-restli/)
