# Prisma Cheat Sheet

![prisma-logo](assets/prisma.svg)

Prisma is an open-source database toolkit that simplifies database access for developers. It provides an Object-Relational Mapping (ORM) layer that allows developers to interact with databases using a type-safe and intuitive API. Prisma supports multiple databases and offers features like data modeling, migrations, and real-time data synchronization.

## Installation

Prisma is a Node.js package that you can install globally via npm:

```bash
$ npm i prisma
```

Initialize a new Prisma project by running the following command in your terminal:

```bash
$ npx prisma init --datasource-provider postgresql
```

Note that a new directory called `prisma` has been created in your project. This directory contains `schema.prisma` which is the main configuration file for your Prisma project, and create a `.env` file with the connection string to your database.

Open up these files and take a look at the content.

> Pro Tip: For a better developer experience, we recommend installing the [Prisma VS Code extension](https://marketplace.visualstudio.com/items?itemName=Prisma.prisma).

## Schema

The Prisma schema is the foundation of your Prisma project. It describes the data model of your application and is used to generate the Prisma Client library. The schema is defined in the `schema.prisma` file in the `prisma` directory.

Our first model is going to be a simple `User` model. Add the following to your `schema.prisma` file:

```prisma
model User {
  id        String   @id @default(autoincrement())
  name      String
  password  String
}
```

## Migrate

Migrations are a way to keep your database schema in sync with your Prisma schema. They are used to create, update, and delete database tables based on your Prisma schema.

To create a migration, run the following command:

```bash
$ npx prisma migrate dev --name init
```

> Make sure that you installed postgresql and created a database before running this command.

This will create a new migration in the `prisma/migrations` directory. Open up the migration file and take a look at the content.

Let's take a deeper look at the prisma schema and the migration file.

- Scalar types:

  - `String`
  - `Int`
  - `Float`
  - `Decimal`
  - `Boolean`
  - `DateTime`
  - `Json`
  - `Bytes`
  - `Decimal`

- Default values:

  - `@default(autoincrement())`
  - `@default(now())`
  - `@default(uuid())`
  - etc.

- Attributes:

  - `@id`
  - `@unique`
  - `@default`
  - `@updatedAt`
  - `@relation`

- Block of Attributes:

  - `@@unique`
  - `@@index`
  - `@@id`

- `??` tells Prisma that this field is optional

### Enums

Enums are a special type that lets you define a list of possible values for a field. They are defined in the `schema.prisma` file and are used to generate the Prisma Client library.

Let's add a new enum called `Role` to our `schema.prisma` file:

```prisma
enum Role {
  GUEST
  USER
  ADMIN
}
```

Now we can use this enum in our `User` model:

```prisma
model User {
  id        String   @id @default(autoincrement())
  name      String
  password  String
  role      Role
}
```

## Prisma Client

The Prisma Client is an auto-generated and type-safe query builder that's tailored to your database schema. It exposes a CRUD API for each model in your schema.

To install the Prisma Client, run the following command:

```bash
$ npm i @prisma/client
```

To generate the Prisma Client, run the following command:

```bash
$ npx prisma generate
```

Now you are able to use the Prisma Client in your application code, create a new file called `index.js` start coding:

- `create` - Create a new record

```ts
import { PrismaClient } from "@prisma/client";

const prisma = new PrismaClient();
async function main() {
  const user = await prisma.user.create({
    data: {
      name: "test",
      email: "test@test.com",
      password: "123",
    },
  });

  console.log(user);
}

main()
  .catch((e) => {
    console.error(e);
  })
  .finally(async () => {
    await prisma.$disconnect();
  });
```

run `ts-node index.ts` and you should see the following output:

```bash
{
  id: 1,
  name: "test",
  email: "test@test.com",
  password: '123',
  createdAt: 2023-07-12T12:32:38.630Z,
  updatedAt: 2023-07-12T12:32:38.630Z
}
```

You have the option to select only specific fields from the database:

```ts
const user = await prisma.user.create({
  data: {
    name: "test",
    email: "test@test.com",
    password: "123",
  },
  select: {
    id: true,
    name: true,
  },
});
```

Look at the console, the user is created but the return value is only the `id` and `name` fields. `{ id: 3, name: 'test' }`

> Tip: If you want to understand how the Prisma Client works under the hood, you can pass the parameter `log: ['query']` to the Prisma Client constructor.

```tsx
const prisma = new PrismaClient({ log: ["query"] });
```

run the code again and you should see sql queries in the console.

- `createMany` - Create multiple records

```ts
const users = await prisma.user.createMany({
  data: [
    {
      name: "test1",
      email: "test1@test.com",
      password: "123",
    },
    {
      name: "test2",
      email: "test2@test.com",
      password: "123",
    },
  ],
});
```

- `findUnique` - Find a single record by id

```ts
const user = await prisma.user.findUnique({
  where: {
    id: 1,
  },
});
```

- `findFirst` - Find the first record

- `findMany` - Find all records

> prisma accepts some parameters to filter the results like `skip : <number>`, `take : <number>`, `orderBy : <object>` or `where : <object>`.

- `update` - Update a record

```ts
const user = await prisma.user.update({
  where: {
    id: 1,
  },
  data: {
    name: "test1",
  },
});
```

- `updateMany` - Update multiple records

```ts
const users = await prisma.user.updateMany({
  where: {
    id: {
      in: [1, 2],
    },
  },
  data: {
    name: "test",
  },
});
```

- `delete` - Delete a record

```ts
const user = await prisma.user.delete({
  where: {
    id: 1,
  },
});
```

- `deleteMany` - Delete multiple records

```ts
const users = await prisma.user.deleteMany({
  where: {
    id: {
      in: [1, 2],
    },
  },
});
```

> To delete all records, you can use the `deleteMany` method without passing any parameters.
