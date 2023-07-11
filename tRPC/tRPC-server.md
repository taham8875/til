# tRPC at Server Side

# Vocabulary

- **Procedure** - API endpoint (query, mutation, subscription)
  - **Query** - Get
  - **Mutation** - Create/Update/Delete
  - **Subscription** - Persistent connection ( perhaps you will use it with websockets )
- **Router** - A collection of procedures under the same namespace
- **Context** - Stuff that every procedure can access. Commonly used for things like session state and database connections.
- **Middleware** - A function that runs before or after procedure. Commonly used for authentication and logging.
- **Validation**

# Give it a try

[Minimal Example](https://stackblitz.com/github/trpc/trpc/tree/main/examples/minimal?file=server%2Findex.ts&file=client%2Findex.ts&view=editor)

# Installation

To install tRPC on the server and on the client, run:

```bash
$ npm install @trpc/server @trpc/client
```

# Getting Started

## Create a router instance

A common convention is to initializa the tRPC backend in a file called `trpc.ts`:

```ts
import { initTRPC } from "@trpc/server";

const t = initTRPC.create(); // only one per backend

export const router = t.router;
export const publicProcedure = t.procedure;
```

jump to `server/index.ts` and import the router and the publicProcedure:to create a router instance:

```ts
import { db } from "./db";
import { router, publicProcedure } from "./trpc";

const appRouter = router({
  hello: publicProcedure.query(async () => {
    // GET
    const users = await db.users.findMany();
    return users;
  }),
});

export type AppRouter = typeof appRouter;
```

Let's implement a route to get a single user by id, but how can we validate that the input id is of a valid type?

- We can use the `input` function
- inside the `input` function we can use the zod library to validate the input

```ts
import { z } from "zod";

const appRouter = router({
  // ...
  getUserById: publicProcedure.input(z.string()).query(async ({ opts }) => {
    const { id } = opts;
    const user = await db.users.findById(id);
    return user;
  }),
});
```

Let's move on to create a `mutation` to create a new user:

```ts
const appRouter = router({
  // ...
  userCreate: publicProcedure
    .input(z.object({ name: z.string() }))
    .mutation(async (opts) => {
      const { input } = opts;

      // Create a new user in the database
      const user = await db.user.create(input);

      return user;
    }),
});
```

Also the method name is different, the implementation of both is very similar.

## Serving the api

Now with out minimal example we are reqady to serve the api, ypu can use any adapter for you favorite framework, (like express), or use the stand alone server.

# Use your backend on the client

Let's connect the client to the server and get the power of end-to-end typesafety without leaking any implementation details to the client.

create a file called `client/index.ts` and initialize the tRPC client:

```ts
import { createTRPCProxyClient, httpBatchLink } from "@trpc/client";
import type { AppRouter } from "../server/trpc";

const trpc = createTRPCProxyClient<AppRouter>({
  links: [
    httpBatchLink({
      url: "http://localhost:3000",
    }),
  ],
});
```

Now you are ready to access the api procedures:

```ts
const users = await trpc.getUserById.query("1");
const createdUser = await trpc.userCreate.mutation({ name: "John" });
```

If you are following along, you must noticed the power of autocompletion introduced by tRPC, type a dot `.` after `trpc` and you will see all the procedures that you can call.

Congratulations🎉! You made your first tRPC app.

# Deep dive

# Server

## Router

To build a tRPC backend, you need to create a router instance. You must initialize the router only once per backend.

`server/trpc.ts`

```ts
import { initTRPC } from "@trpc/server";

const t = initTRPC.create();

export const router = t.router;
export const middlewares = t.middlewares;
export const publicProcedure = t.procedure;
```

### Define a router

`server/_app.ts`

```ts
import * as trpc from "@trpc/server";
import { publicProcedure, router } from "./trpc";

const appRouter = router({
  hello: publicProcedure.query(() => {
    return "Hello World";
  }),
});

// export the router type only, not the router itself

export type AppRouter = typeof appRouter;
```

## Procedure

Procedures are similar to API endpoints in REST. they are functions exposed by the server that can be called by the client.

They can be of three types:

- **Query** - Get
- **Mutation** - Create, Update, Delete
- **Subscription** - Persistent connection ( perhaps you will use it with websockets ), but you might not need it.

## Write a procedure

The `t` object you create during tRPC setup returns an initial `t`.procedure which all other procedures are built on:

```ts
import { initTRPC } from "@trpc/server";
import { z } from "zod";
import { publicProcedure } from "./trpc";
// remember how `publicProcedure` was created in the previous section?
// export const publicProcedure = t.procedure;

const appRouter = router({
  hello: publicProcedure.query(async () => {
    return "Hello!";
  }),
  userCreate: publicProcedure
    .input(z.object({ name: z.string(), age: z.number() }))
    .mutation(async (opts) => {
      const { input } = opts;

      // Create a new user in the database
      const user = await db.user.create(input);

      return user;
    }),
});
```

### Output validation

Validating outputs is not always as important as defining inputs, since tRPC gives you automatic type-safety by inferring the return type of your procedures. But if you wanted to be explicit, you can use the `.output()` method:

```ts
import { z } from "zod";

export const t = initTRPC.create();
const publicProcedure = t.procedure;

export const appRouter = t.router({
  hello: publicProcedure
    .output(
      z.object({
        greeting: z.string(),
      })
    )
    .query((opts) => {
      return {
        greeting: "Hello!",
      };
    }),
});
```

The most basic validation you can do is writing a function:

```ts
.output((value): string => {
    if (typeof value === 'string') {
      return value;
    }
    throw new Error('Output is not a string');
  })
```

However, in most cases it is recommended to use a library like zod

```ts
.output(z.string())
```

### Merging Routers

In greater project, it is not recommended to have all your procedures in one file, you can split them into multiple files and merge them together:

```ts
// @filename: routers/user.ts
// ...
export const userRouter = router({
  helloFromUser: publicProcedure.query(() => {
    return "Hello From User!";
  }),
});

// @filename: routers/post.ts
// ...
export const postRouter = router({
  helloFromPost: publicProcedure.query(() => {
    return "Hello From Post!";
  }),
});
```

Now in your main router file: `trpc.ts`

```ts
// ...
const appRouter = router({
  hello: publicProcedure.query(async () => {
    return "Hello!";
  }),
  user: userRouter,
  post: postRouter,
});
```

If you want your procedures flat in one single namespace( same level ), you can use the `t.mergeRouters` function:

```ts
// @filename: trpc.ts
// ...
export const mergedRouter = t.mergeRouters();

// @filename: routers/_app.ts
// ...
import { ..., mergedRouter } from "../trpc";
import { userRouter } from './user';
import { postRouter } from './post';

const appRouter = mergeRouters(userRouter, postRouter)
```

## Context

Your context holds data that all of your tRPC procedures will have access to, and is a great place to put things like database connections or authentication information.

### Define a context type

When initializing tRPC using `initTRPC`, you should pipe the `.context<TContext>` to before calling the `create` function.

Thy type can be explicitly defined or be inferred from the `createContext` function:

```ts
import { initTRPC, type inferAsyncReturnType } from '@trpc/server';
import type { CreateNextContextOptions } from '@trpc/server/adapters/next';
import { getSession } from 'next-auth/react';

export const createContext = async (opts: CreateNextContextOptions) => {
  const session = await getSession({ req: opts.req });

  return {
    session, // 👈 this is the context
  };
};

const t1 = initTRPC.context<typeof createContext>().create();
t1.procedure.use(({ ctx }) => { ... });

type Context = inferAsyncReturnType<typeof createContext>;

const t2 = initTRPC.context<Context>().create();
t2.procedure.use(({ ctx }) => { ... });
```

Now we can use the `createContext` function to create our context, let'a create a `isAuthed` middleware that will be used by all our procedures needing authentication:

```ts
// @filename: context.ts
// ...
export async function createContext(opts: CreateNextContextOptions) {
  const session = await getSession({ req: opts.req });

  return {
    session,
  };
}

export type Context = inferAsyncReturnType<typeof createContext>;

// @filename: trpc.ts
import { initTRPC, TRPCError } from "@trpc/server";
import { Context } from "./context";

const t = initTRPC.context<Context>().create();

const isAuthed = t.middleware(({ next, ctx }) => {
  if (!ctx.session?.user?.email) {
    throw new TRPCError({
      code: "UNAUTHORIZED",
    });
  }
  return next({
    ctx: {
      // Infers the `session` as non-nullable
      session: ctx.session,
    },
  });
});

export const middleware = t.middleware;
export const router = t.router;

/**
 * Unprotected procedure, used for public data
 */
export const publicProcedure = t.procedure;

/**
 * Protected procedure, used for private data
 */
export const protectedProcedure = t.procedure.use(isAuthed); // 👈 using the middleware, like we always do in express
```

## Middleware

Middleware is a function that runs before your procedure. It can be used for things like authentication, logging, etc.

### Middleware for Authorization

In this example, let call the `adminProcedure` which ensures that the user is an admin:

```ts
import { initTRPC, TRPCError } from "@trpc/server";

interface Context {
  user?: {
    // [...]
    isAdmin: boolean;
  };
}

const t = initTRPC.context<Context>().create();
export const middleware = t.middleware;
export const publicProcedure = t.procedure;
export const router = t.router;

const isAdmin = middleware(async (opts) => {
  const { ctx } = opts;
  if (!ctx.user?.isAdmin) {
    throw new TRPCError({ code: "UNAUTHORIZED" });
  }
  return opts.next({
    ctx: {
      user: ctx.user,
    },
  });
});

export const adminProcedure = publicProcedure.use(isAdmin);
```

```ts
import { adminProcedure, publicProcedure, router } from './trpc';

const adminRouter = router({
  secretPlace: adminProcedure.query(() =>
  return 'Hello from the secret place!'
  ),
});

export const appRouter = router({
  foo: publicProcedure.query(() => 'bar'),
  admin: adminRouter,
});
```

## Adapter

tRPC is not a standalone server, it needs to be plugged into an existing server such as express. This is done using an adapter. Adapters act as the glue between the host system and your tRPC API.

### Express

- Install dependencies:

```bash
yarn add @trpc/server zod
```

- Create a simple `appRouter.ts` file:

```ts
import { initTRPC } from "@trpc/server";
import { z } from "zod";
export const t = initTRPC.create();
export const appRouter = t.router({
  getUser: t.procedure.input(z.string()).query((opts) => {
    opts.input; // string
    return { id: opts.input, name: "Bilbo" };
  }),
  createUser: t.procedure
    .input(z.object({ name: z.string().min(5) }))
    .mutation(async (opts) => {
      // use your ORM of choice
      return await UserModel.create({
        data: opts.input,
      });
    }),
});
// export type definition of API
export type AppRouter = typeof appRouter;
```

If you have multiple routers, implement each router separately and then mere them into a single router:

```ts
import { inferAsyncReturnType, initTRPC } from "@trpc/server";
import * as trpcExpress from "@trpc/server/adapters/express";

// created for each request
const createContext = ({
  req,
  res,
}: trpcExpress.CreateExpressContextOptions) => ({}); // no context
type Context = inferAsyncReturnType<typeof createContext>;

const t = initTRPC.context<Context>().create();
const appRouter = t.router({
  // [...]
});

const app = express();

app.use(
  "/trpc",
  trpcExpress.createExpressMiddleware({
    router: appRouter,
    createContext,
  })
);

app.listen(4000);
```

Congratulations! You now have a working tRPC server. The endpoints are now available via HTTP:

| Endpoint     | HTTP URI                                                                                |
| ------------ | --------------------------------------------------------------------------------------- |
| `getUser`    | `GET http://localhost:4000/trpc/getUser?input=INPUT`                                    |
| `createUser` | `POST http://localhost:4000/trpc/createUser` with `req.body` of type `{ name: string }` |

# Server Side Calls

tRPC supports server side calls. This means that you can call your API from within your server hosting it, `router.createCaller()` can be used.

> Note that `router.createCaller()` shouldn't be used to call procedures inside other procedures. this creating an overhead by potentially creating constant again, executing all middleware and validation, all of this were already done in the original call.

## Create caller

With the `router.createCaller({})` function (first argument is `Context`) we retrieve an instance of `RouterCaller`.

## Example

input example

```ts
import { initTRPC } from "@trpc/server";
import { z } from "zod";
const t = initTRPC.create();
const router = t.router({
  // Create procedure at path 'greeting'
  greeting: t.procedure
    .input(z.object({ name: z.string() }))
    .query((opts) => `Hello ${opts.input.name}`),
});
const caller = router.createCaller({});
const result = await caller.greeting({ name: "tRPC" });
console.log(result); // Hello tRPC
```

mutation example

```ts
import { initTRPC } from "@trpc/server";
import { z } from "zod";

const posts = ["One", "Two", "Three"];

const t = initTRPC.create();
const router = t.router({
  post: t.router({
    add: t.procedure.input(z.string()).mutation((opts) => {
      posts.push(opts.input);
      return posts;
    }),
  }),
});

const caller = router.createCaller({});
const result = await caller.post.add("Four");
console.log(result); // ['One', 'Two', 'Three', 'Four']
```

with middleware example

```ts
import { initTRPC, TRPCError } from "@trpc/server";

type Context = {
  user?: {
    id: string;
  };
};
const t = initTRPC.context<Context>().create();

const isAuthed = t.middleware((opts) => {
  const { ctx } = opts;
  if (!ctx.user) {
    throw new TRPCError({
      code: "UNAUTHORIZED",
      message: "You are not authorized",
    });
  }

  return opts.next({
    ctx: {
      // Infers that the `user` is non-nullable
      user: ctx.user,
    },
  });
});

const protectedProcedure = t.procedure.use(isAuthed);

const router = t.router({
  secret: protectedProcedure.query((opts) => opts.ctx.user),
});

{
  // ❌ this will return an error because there isn't the right context param
  const caller = router.createCaller({});

  const result = await caller.secret();
}

{
  // ✅ this will work because user property is present inside context param
  const authorizedCaller = router.createCaller({
    user: {
      id: "KATT",
    },
  });
  const result = await authorizedCaller.secret();
  console.log(result); // { id: 'KATT' }
}
```

# Authorization

The `createContext` function is called for each request. so you can add contextual information to the request. This is useful for authorization.

## Create context from request headers

`server/context.ts`

```ts
import * as trpc from "@trpc/server";
import { inferAsyncReturnType } from "@trpc/server";
import * as trpcNext from "@trpc/server/adapters/next";
import { decodeAndVerifyJwtToken } from "./somewhere/in/your/app/utils";

export async function createContext({
  req,
  res,
}: trpcNext.CreateNextContextOptions) {
  // Create your context based on the request object
  // Will be available as `ctx` in all your resolvers

  // This is just an example of something you might want to do in your ctx fn
  async function getUserFromHeader() {
    if (req.headers.authorization) {
      const user = await decodeAndVerifyJwtToken(
        req.headers.authorization.split(" ")[1]
      );
      return user;
    }
    return null;
  }
  const user = await getUserFromHeader();

  return {
    user,
  };
}
export type Context = inferAsyncReturnType<typeof createContext>;
```

### Option 1 : Authorize using resolver

`server/router/_app.ts`

```ts
import { initTRPC, TRPCError } from "@trpc/server";
import type { Context } from "../context";
export const t = initTRPC.context<Context>().create();
const appRouter = t.router({
  // open for anyone
  hello: t.procedure
    .input(z.string().nullish())
    .query((opts) => `hello ${opts.input ?? opts.ctx.user?.name ?? "world"}`),
  // checked in resolver
  secret: t.procedure.query((opts) => {
    if (!opts.ctx.user?.isAdmin) {
      throw new TRPCError({ code: "UNAUTHORIZED" });
    }
    return {
      secret: "sauce",
    };
  }),
});
```

### Option 2 : Authorize using middleware

`server/router/_app.ts`

```ts
import { initTRPC, TRPCError } from "@trpc/server";

export const t = initTRPC.context<Context>().create();

const isAuthed = t.middleware((opts) => {
  const { ctx } = opts;
  if (!ctx.user?.isAdmin) {
    throw new TRPCError({ code: "UNAUTHORIZED" });
  }
  return opts.next({
    ctx: {
      user: ctx.user,
    },
  });
});

// you can reuse this for any procedure
export const protectedProcedure = t.procedure.use(isAuthed);

t.router({
  // this is accessible for everyone
  hello: t.procedure
    .input(z.string().nullish())
    .query((opts) => `hello ${opts.input ?? opts.ctx.user?.name ?? "world"}`),
  admin: t.router({
    // this is accessible only to admins
    secret: protectedProcedure.query((opts) => {
      return {
        secret: "sauce",
      };
    }),
  }),
});
```

# Error handling

Whenever an error occurs in a procedure, tRPC responds to the client with an object that includes an "error" property. This property contains all the information that you need to handle the error in the client.

```json
{
  "id": null,
  "error": {
    "message": "\"password\" must be at least 4 characters",
    "code": -32600,
    "data": {
      "code": "BAD_REQUEST",
      "httpStatus": 400,
      "stack": "...",
      "path": "user.changepassword"
    }
  }
}
```

## Error codes

---

| Code                    | Description                                                  | HTTP Status |
| ----------------------- | ------------------------------------------------------------ | ----------- |
| `BAD_REQUEST`           | client error                                                 | 400         |
| `UNAUTHORIZED`          | Not valid credentials, I don't know how you are              | 401         |
| `FORBIDDEN`             | Not valid credentials, I know you but you shouldn't see that | 403         |
| `NOT_FOUND`             | Not found                                                    | 404         |
| `METHOD_ERROR`          | You are using the wrong HTTP method                          | 405         |
| `TOO_MANY_REQUESTS`     | You are doing too many requests , rate limit exceeded        | 429         |
| `INTERNAL_SERVER_ERROR` | Something went wrong on the server side                      | 500         |

The full list of error codes can be found [here](https://trpc.io/docs/server/error-handling#error-codes)

tRPC exposes a helper function, `getHTTPStatusCodeFromError`, to help you extract the HTTP code from the error:

```ts
import { getHTTPStatusCodeFromError } from "@trpc/server/http";
// Example error you might get if your input validation fails
const error: TRPCError = {
  name: "TRPCError",
  code: "BAD_REQUEST",
  message: '"password" must be at least 4 characters',
};
if (error instanceof TRPCError) {
  const httpCode = getHTTPStatusCodeFromError(error);
  console.log(httpCode); // 400
}
```

## Throwing errors

tRPC provides an error subclass, `TRPCError`, which you can use to represent an error that occurred inside a procedure.

`server.ts`

```ts
import { initTRPC, TRPCError } from "@trpc/server";

const t = initTRPC.create();

const appRouter = t.router({
  hello: t.procedure.query(() => {
    if (badLogic) {
      throw new TRPCError({
        code: "INTERNAL_SERVER_ERROR",
        message: "An unexpected error occurred, please try again later.",
        // optional: pass the original error to retain stack trace
        cause: theError,
      });
    } else {
      // ...
    }
  }),
});

// [...]
```

Results in the following response:

```json
{
  "id": null,
  "error": {
    "message": "An unexpected error occurred, please try again later.",
    "code": -32603,
    "data": {
      "code": "INTERNAL_SERVER_ERROR",
      "httpStatus": 500,
      "stack": "...",
      "path": "hello"
    }
  }
}
```

## Handling errors

All errors that occur in a procedure go through the onError method before being sent to the client. Here you can handle or change errors.

`pages/api/trpc/[trpc].ts`

```ts
export default trpcNext.createNextApiHandler({
  // ...
  onError(opts) {
    const { error, type, path, input, ctx, req } = opts;
    console.error("Error:", error);
    if (error.code === "INTERNAL_SERVER_ERROR") {
      // send to bug reporting
    }
  },
});
```

# Metadata

tRPC supports adding metadata to your procedures. This is useful for adding extra information about your procedures, such as the description, tags, or even the HTTP method.

Create a router with `authRequired` metadata:

```ts
import { initTRPC } from "@trpc/server";
// [...]
interface Meta {
  authRequired: boolean;
}
export const t = initTRPC.context<Context>().meta<Meta>().create();
export const appRouter = t.router({
  // [...]
});
```

Now you can use a middleware to check if the procedure requires authentication, if so authenticate the user, otherwise continue to the next procedure.

```ts
import { initTRPC } from "@trpc/server";
// [...]
interface Meta {
  authRequired: boolean;
}
export const t = initTRPC.context<Context>().meta<Meta>().create();
const isAuthed = t.middleware(async (opts) => {
  const { meta, next, ctx } = opts;
  // only check authorization if enabled
  if (meta?.authRequired && !ctx.user) {
    throw new TRPCError({ code: "UNAUTHORIZED" });
  }
  return next();
});
export const authedProcedure = t.procedure.use(isAuthed);
export const appRouter = t.router({
  hello: authedProcedure.meta({ authRequired: false }).query(() => {
    return {
      greeting: "hello world",
    };
  }),
  protectedHello: authedProcedure.meta({ authRequired: true }).query(() => {
    return {
      greeting: "hello-world",
    };
  }),
});
```

> Note you can set default metadata for all procedures in a router by using the method `defaultMeta` inside the `initTRPC.create()` function.

```ts
import { initTRPC } from '@trpc/server';

interface Meta {
  authRequired: boolean;
  role?: 'user' | 'admin'
}

export const t = initTRPC
  .context<Context>()
  .meta<Meta>()
  .create({
    // Set a default value
    defaultMeta: { authRequired: false }
  });

const publicProcedure = t.procedure
// ^ Default Meta: { authRequired: false }

const authProcedure = publicProcedure
  .use(authMiddleware)
  .meta({
    authRequired: true;
    role: 'user'
  });
// ^ Meta: { authRequired: true, role: 'user' }

const adminProcedure = authProcedure
  .meta({
    role: 'admin'
  });
// ^ Meta: { authRequired: true, role: 'admin' }
```
