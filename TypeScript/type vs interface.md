# `type` vs `interface`

Both `type` and `interface` are used to define a type in TypeScript. So what's the difference between them?

## How to define a type

```ts
type userTypes = {
  name: string;
  age: number;
};
```

## How to define an interface


```ts
interface userInterface {
  name: string;
  age: number;
}
```

## usage

There are no differences in usage between `type` and `interface`. They can be used interchangeably.

```ts
const user1: userTypes = {
  name: 'John',
  age: 18,
};

const user2: userInterface = {
  name: 'John',
  age: 18,
};
```

So why `type` is better than `interface`?

## Reason 1: `type` can be used to define a single type, while `interface` is for defining an object type only.

```ts
type userTypes = string;

const user1: userTypes = 'John';

interface userInterface {
  name: string;
  age: number;
}

const user2: userInterface = 'John'; // error
```

## Reason 2: `type` can be used to define a union type, `interface` can also be used to define an intersection type.

```ts
type phone = string | number;

// this cannot be done with interface

type user = { name: string } | { age: number }; // either name or age is okay

// this cannot be done with interface
```


Anding types is also possible with `type`:

```ts
type user1 = { name: string }

type user2 = { age: number }

type user = user1 & user2; // both name and age are required

// this can be done with interface

interface user1 { name: string }

interface user2 { age: number }

interface user extends user1, user2 {} // both name and age are required
```

## The only benefit of `interface` is that it can be used to extend other interfaces.

```ts
interface user {
    name: string;
}

interface user {
    age: number;
}

const user: user = {
    name: 'John',
    age: 18,
}

const user: user = {
    name: 'John',
} // error ❌
```

for example, you can extend `Window` interface to add your own properties.