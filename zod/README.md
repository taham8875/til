# Zod Cheat Sheet

Zod is a TypeScript-first schema declaration and validation library.

## Installation

```bash
$ npm install zod
```

## Minimal Example

```ts
import { z } from "zod";

const UserSchema = z.object({
  name: z.string(),
  age: z.number(),
});

const user = {
  name: "John Doe",
  age: 18,
};

console.log(UserSchema.parse(user));
```

In this example, we define a schema for a user object. We then parse the user object using the schema. If the user object is valid, the parsed object is returned. If the user object is invalid, an error is thrown.

- If you want not to throw and error (you just want to simply know if the object is valid or not), you can use the `safeParse` method instead of `parse`.

```ts
const result = UserSchema.safeParse(user);
if (result.success) {
  console.log("User is valid!");
} else {
  console.log("User is invalid. Enter a valid user.");
}
```

- If you want to use types, you can use the `infer` method to infer the type of the schema.

Instead of this:

```ts
type User = {
  name: string;
  age: number;
};

const UserSchema = z.object({
  name: z.string(),
  age: z.number(),
});
```

Do this:

```ts
const UserSchema = z.object({
  name: z.string(),
  age: z.number(),
});

type User = z.infer<typeof UserSchema>;

const user: User = {
  name: "John Doe",
  age: 18,
};
```

This helps you to avoid repeating definitions.

# Other data types

```ts
const UserSchema = z.object({
  name: z.string(),
  age: z.number(),
  isStudent: z.boolean(),
  birthDate: z.date(),
  someField: z.any(),
  favoriteFoods: z.array(z.string()),
  favoriteNumbers: z.tuple([z.number(), z.number()]),
  favoriteColors: z.enum(["red", "green", "blue"]),
)}
```

# Optional fields

Note that when creating zod objects, all fields are required by default. If you want to make a field optional, you can use the `optional` method.

```ts
const UserSchema = z.object({
  name: z.string(),
  age: z.number(),
  isStudent: z.boolean().optional(),
  birthDate: z.date().optional(),
  someField: z.any().optional(),
  favoriteFoods: z.array(z.string()),
  favoriteNumbers: z.tuple([z.number(), z.number()]),
  favoriteColors: z.enum(["red", "green", "blue"]),
)}
```

if you want to make all fields optional, you can use the `partial` method.

```ts
const UserSchema = z
  .object({
    name: z.string(),
    age: z.number(),
    isStudent: z.boolean(),
    birthDate: z.date(),
    someField: z.any(),
  })
  .partial();
```

If you have a nested object, you can use the `deepPartial` method.

```ts
const UserSchema = z
  .object({
    name: z.string(),
    age: z.number(),
    isStudent: z.boolean(),
    birthDate: z.date(),
    someField: z.any(),
    address: z.object({
      street: z.string(),
      city: z.string(),
      country: z.string(),
    }),
  })
  .deepPartial();
```

# Additional validation

As we saw in the previous example, we can use the `optional` method to make a field optional. This is applicable to any field type, but some fields have additional validation methods.

For example, we can use the `min` and `max` methods to validate the minimum and maximum length of a string.

also, `gt` and `lt` can be used to validate numbers.

```ts
const UserSchema = z.object({
  name: z.string().min(3).max(20),
  age: z.number().g
  isStudent: z.boolean().optional(),
  birthDate: z.date().optional(),
  someField: z.any().optional(),
});
```

`nullable` can be used to allow `null` values, and `nullish` can be used to allow `null` and `undefined` values.

```ts
const UserSchema = z.object({
  isStudent: z.boolean().nullable(),
});
```

We can create a `default` value for a field using the `default` method.

```ts
const UserSchema = z.object({
  isStudent: z.boolean().default(false),
});
```

If you didn't provide a value for the field, the default value will be used.

You also can pass a function to the `default` method. The function will be called if you didn't provide a value for the field.

```ts
const UserSchema = z.object({
  isStudent: z.boolean().default(Math.random()),
});
```

Enums can be used to validate a field against a set of values.

```ts
const UserSchema = z.object({
  favoriteColor: z.enum(["red", "green", "blue"]),
});

const user = {
  favoriteColor: "red",
};
```

If you want to extract the enum array out of the schema definition, zod will complain that the enum array mutable. To fix that, we must use the following syntax:

```ts
const colors = ["red", "green", "blue"] as const;

const UserSchema = z.object({
  favoriteColor: z.enum(colors),
});
```

If you want to use typescript enums, you can use the `nativeEnum` method.

```ts
enum Color {
  Red = "red",
  Green = "green",
  Blue = "blue",
}

const UserSchema = z.object({
  favoriteColor: z.nativeEnum(Color),
});

const user = {
  favoriteColor: Color.Red,
};
```

We can get the shape of an object using the `shape` method.

```ts
const UserSchema = z.object({
  name: z.string(),
  age: z.number(),
});
```

## Extend and Merge

If you want to extend an existing schema, you can use the `extend` method.

```ts
const UserSchema = z.object({
  name: z.string(),
  age: z.number(),
});

const ExtendedUserSchema = UserSchema.extend({
  isStudent: z.boolean(),
});
```

`merge` can be used to merge two schemas.

```ts
const UserSchema = z.object({
  name: z.string(),
  age: z.number(),
});

const AddressSchema = z.object({
  street: z.string(),
  city: z.string(),
  country: z.string(),
});

const UserWithAddressSchema = UserSchema.merge(AddressSchema);
```

## Additional fields

By default, zod will pass any additional fields. If you want to read additional fields, you can use the `passthrough` method.

```ts
const UserSchema = z
  .object({
    name: z.string(),
    age: z.number(),
  })
  .passthrough();

const user = {
  name: "John Doe",
  age: 18,
  isStudent: true,
};

const validatedUser = UserSchema.parse(user);

console.log(validatedUser.isStudent); // true
```

And if you want to forbid additional fields, you can use the `strict` method.

```ts
const UserSchema = z
  .object({
    name: z.string(),
    age: z.number(),
  })
  .strict();

const user = {
  name: "John Doe",
  age: 18,
  isStudent: true,
};

const validatedUser = UserSchema.parse(user); // throws an error
```

## Multi-type fields (Unions)

If a field can have multiple types, you can use the `union` method.

```ts
const UserSchema = z.object({
  name: z.string(),
  age: z.union([z.number(), z.string()]),
});
```

A discriminated union is a union of object schemas that all share a particular key.

```ts
const myUnion = z.discriminatedUnion("status", [
  z.object({ status: z.literal("success"), data: z.string() }),
  z.object({ status: z.literal("failed"), error: z.instanceof(Error) }),
]);

myUnion.parse({ status: "success", data: "yippie ki yay" });
```

## Maps (key-value pairs)

Yo define the schema for maps (key-value pairs), you can use the `record` method.

```ts
const UserMap = z.record(z.string(), z.number());
// another one
const PostMap = z.record(
  z.string(),
  z.object({
    title: z.string(),
    body: z.string(),
    author: z.string(),
  })
);
```

## Promises

Promises can be validated using the `promise` method.

```ts
const PromiseSchema = z.promise(z.string());

const promise1 = Promise.resolve("Hello World");
const promise2 = Promise.resolve(42);

console.log(PromiseSchema.parse(promise1)); // success
console.log(PromiseSchema.parse(promise2)); // throws an error
```

# Custom validation

You can use the `refine` method to add custom validation to a field.

```ts
const EmailSchema = z.string().refine((email) => {
  return email.endsWith("@gmail.com");
}, "Invalid email, must be a gmail account");

const email = "test@test.com";

console.log(EmailSchema.parse(email)); // throws an error
```

# Custom error messages

There are the option to write custom error messages for each field.

```ts
const UserSchema = z.object({
  name: z.string().min(3, "Name must be at least 3 characters"),
  age: z.number().max(20, "Age must be less than 20"),
});
```

However, You can use custom libraries to write custom error messages.

```bash
$ npm install zod-validation-error
```

```ts
import { fromZodError } from "zod-validation-error";

// code

const result = UserSchema.safeParse(user);

if (result.success) {
  // code
} else {
  console.log(fromZodError(result.error));
}
```
