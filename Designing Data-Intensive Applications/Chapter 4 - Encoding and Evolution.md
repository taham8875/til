# Chapter 4 - Encoding and Evolution

# Introduction

- Applications evolves over time: this may be due to new features, bug fixes, performance improvements, better understood business logic, etc.
- **Evolvability** is the ability to make changes to the application easily.
- In most cases, changes to the application requires changes to the data stored in the database.
- Relational databases have it own ways to handle schema changes (i.e. `ALTER TABLE`).
- Such a change in the database schema requires a corresponding change in the application code.
- In large applications, it is not possible to make all the changes at once.
  - In *server side application* We need to make a *rolling upgrade*, deploy the new code to a few servers, then gradually roll out the new code to all servers until migrating completely to the new version without downtime.
  - In *client side application* god helps you, users rarely update their applications.
- This means the both old and new code coexists in the same system at the same time.
- This forces us to maintain:
  - **Backward compatibility**: new code can read data written by old code. (easier to achieve)
  - **Forward compatibility**: old code can read data written by new code. (a little bi tricky)

# Formats for Encoding Data

1. In memory data structures, like objects, arrays, hash tables, trees, etc.
2. When you want to save data to disk or send it over the network, you have to encode it as some kind of self-contained sequence of bytes (i.e. *serialization*, *encoding*, or *pickling*) , for example (JSON, XML, Protocol Buffers)

## JSON

JSON is widely used for encoding data for communication between a web browser and a server.

Advantages:

- JSON is simple and easy to implement.
- JSON has a human readable encoding.
- JSON has implementations in many languages.
- JSON is subset of JavaScript, and readable by web browsers.
- JSON is very very popular and widely used.

The last one is the mane and most powerful advantage of JSON, as long as people agree on what the format is, it often doesn’t matter how pretty or efficient the format is. The difficulty of getting different organizations to agree on anything outweighs most other concerns.

For data the is used internally within an organization, there is less pressure to stick with a standard format, and you can choose a more efficient encoding.

Also efficient data encoding benefits are negligible for small data, but it becomes important for large data.

Disadvantages:

- Ambiguity of representing numbers, no distinguish between integers and floating point numbers.
- Problematic for encoding large numbers.
- No support for binary data. (a workaround is to encode binary data as a string using Base64 encoding)
- JSON is verbose, there are many repeated keys for example and syntax is not very concise
- Overfetching: when you request a resource, you get more information than you need. (a workaround is to use GraphQL)

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

# Protocol Buffers

Refer to my [Protocol Buffers notes](https://github.com/taham8875/til/tree/main/Protocol%20Buffers)

# Advantages of binary encodings

although textual data formats such as JSON, XML, and CSV are widespread, binary encodings based on schemas are also a viable option. They have a number of nice properties:

- More compact size (less storage space, less bandwidth, less CPU)
- Schema is a valuable form of documentation (and you can be sure that it is up to date, unlike manually written documentation which can be outdated)
- Keeping a database of schemas help you to check backward and forward compatibility before deploying a new version of the application
- Developers with statically types languages can generate code from the schema, which gives them a nice statically typed interface to work with with auto completion and type checking at compile time which prevents many bugs
