# Chapter 1 - About Node.js

## Overview of Node.js

- Node.js is a JavaScript runtime built on Chrome's V8 JavaScript engine.

- The key architectural choice is that Node.js is event-driven, rather than multithreaded. The Node.js architecture rests on dispatching blocking operations to a single-threaded event loop, with results arriving back to the caller as an event that invokes an event handler function.

## The capabilities of Node.js

While Node.js executes the same JavaScript language that we use in browsers, it doesn't have some of the features associated with the browser. For example, there is no HTML DOM built into Node.js.

Instead of using Node.js to work at low level of the HTTP protocol, we can use web frameworks, such as Express framework to build web applications.

## What are folks doing with Node.js?

- Build tools (Grunt, Gulp, Webpack, PostCSS)
- Web UI testing (Puppeteer)
- Desktop apps (Electron)
- Mobile apps
- IoT

## Why should you use Node.js?

- Popularity
- JavaScript everywhere
- Leveraging Google's investment in V8
- Leaner, asynchronous, event-driven programming
- Microservice architecture
- Node.js is stronger after a major schism and hostile

## The Node.js event-driven architecture

Node.js's good performance is because of its asynchronous event driven architecture. This enables it to handle multiple tasks concurrently, such as juggling between requests from multiple web browsers.

A single thread, event-driven programming model is simpler to code and has less complexity and less overhead than application servers that rely on threads to handle multiple concurrent tasks.

By converting blocking function calls into asynchronous code execution, you can configure the systems so that it issues an event when the blocking request is satisfied.

## The Node.js answer to complexity

Callbacks fired asynchronously from an event loop are much simpler.

Node.js has a single execution thread. If the goal is to avoid the complexity of a multithreaded system, then the system must use asynchronous operations as Node.js does.

## Asynchronous requests in Node.js

```javascript
queryDatabase("SELECT * FROM users WHERE id = 1", function (err, result) {
  if (err) {
    console.error(err);
    return;
  }
  console.log(result);
});
```

The programmer supplies a function that is called (hence the name callback function) when the result (or error) is available. The query function still takes the same amount of time. Instead of blocking the execution thread, it returns to the event loop, which is then free to handle other requests. The Node.js will eventually fire an event that causes this callback function to be called with the result or error indication.

Advances in the JavaScript language have given us new options. When used with ES2015 promises, the equivalent code would look like this:

```javascript
queryDatabase("SELECT * FROM users WHERE id = 1")
  .then(function (result) {
    console.log(result);
  })
  .catch(function (err) {
    console.error(err);
  });
```

The big advance came with the ES-2017 async function:

```javascript
try {
  const result = await queryDatabase("SELECT * FROM users WHERE id = 1");
  console.log(result);
} catch (err) {
  console.error(err);
}
```

All three of these code snippets perform the same query that we wrote earlier. Instead of query being a blocking function call, it is asynchronous and does not block the execution thread.

## Is Node.js a cancerous scalability disaster?

It is important to consider the following question: where do you put the heavy computational tasks?

A key to maintaining high throughput of Node.js applications is by ensuring that events are handled quickly. Because it uses a single execution thread, if that thread is bogged down with a big calculation, Node.js cannot handle events, and event throughput will suffer.

For example, a Fibonacci calculation is a good example of a CPU-intensive task. If you run this code (nobody implements Fibonacci that way), you will see that the event loop is blocked for the duration of the calculation.

```javascript
function fibonacci(n) {
  if (n < 2) {
    return 1;
  }
  return fibonacci(n - 2) + fibonacci(n - 1);
}
```

Embedding this function into an HTTP server, we can see that for bigger `n` (e.g. 40), the server becomes completely unresponsive because the event loop is not running. Instead, this function has blocked event processing because the event loop cannot dispatch events while the function is grinding through the calculation.

Does this mean that Node.js is a flawed platform? No, it just means that the programmer must take care to identify code with long-running computations and develop solutions. These include rewriting the algorithm to work with the event loop, rewriting the algorithm for efficiency, integrating a native code library, or foisting computationally expensive calculations to a backend server.

## Embracing advances in the JavaScript language

Javascript frontend developers usually unable to use the latest JavaScript language features because of the need to support older browsers like old ie browsers, they need to use tools like Bable to transpile the code to older JavaScript versions.

Node.js developers are not constrained by this requirement, they can use the latest JavaScript language features as soon as they are implemented in V8.

## TypeScript and Node.js

TypeScript is a superset of JavaScript that adds static typing and other features to the language. TypeScript is a language that is compiled to JavaScript. TypeScript is a superset of JavaScript, so any valid JavaScript code is also valid TypeScript code, but the reverse is not true.

TypeScript ease the pain of using javascript in large and complex projects, strong type checking and other features help to catch errors early in the development process.

## Developing microservices or maxiservices with Node.js

Microservices are a way of designing applications as a collection of small services, each of which runs its own process and communicates with lightweight mechanisms, often an HTTP resource API. Each microservice is a separate process that can be deployed, upgraded, and scaled independently.

Instead of building a single monolithic application, we can build a collection of microservices that work together to provide the same functionality.

Some advantages of microservices are as follows:

- Each microservice can be managed by a small team.
- Each team can work on its own schedule, so long as the service API compatibility is maintained.
- Each microservice can be written in a different language.
- Microservices can be deployed independently should this be required, such as for easier testing.
- It's easier to switch technology stack choices.

Where does Node.js fit in with this? Its design fits the microservice :

- Node.js encourages small, tightly focused, single-purpose modules.
- These modules are composed into an application by the excellent npm package management system.
- Publishing modules is incredibly simple, whether via the NPM repository or a Git URL.
- While an app framework such as Express can be used with large services, it works very well for small lightweight services and supports easy, simple deployment.

# Chapter 2: Setting up Node.js

## Installing Node.js

Node js : https://nodejs.org/en/download

Editor : https://code.visualstudio.com/

to check if node is installed, run the following command in terminal

```bash
$ node -v
v18.15.0
```

To get the help for node, run the following command in terminal

```bash
$ node --help
```

To open the interactive node shell, run the following command in terminal

```bash
$ node
Welcome to Node.js v18.15.0.
Type ".help" for more information.
> console.log('Hello Node')
Hello Node
undefined
```

To exit the interactive node shell, either press `ctrl+c` twice or type `.exit` and press enter.

## Running a simple script with Node.js

Create a file called `ls.js` , which will list the files in the current directory like the `ls` command in linux dose.

```javascript
const fs = require("fs").promises;
async function listFiles() {
  try {
    const files = await fs.readdir(".");
    for (const file of files) {
      console.log(file);
    }
  } catch (err) {
    console.error(err);
  }
}
listFiles();
```

To run the script, run the following command in terminal

```bash
$ node ls.js
```

> For the sake of learning, I will use TypeScript in the rest of the document, but you can use JavaScript if you prefer to be consistent with the book.

```ts
import fs from "fs/promises";

async function ls(): Promise<void> {
  try {
    const files: string[] = await fs.readdir(".");
    files.forEach((file) => process.stdout.write(`${file}   `));
  } catch (err) {
    console.log(err);
  }
}

ls();
```

To run the script, run the following command in terminal

```bash
$ ts-node ls.ts
```

[`ts-node`](https://github.com/TypeStrong/ts-node) is a TypeScript execution engine and REPL for Node.js.

> By default, the fs module functions use the callback paradigm originally created for Node.js. As a result, most Node.js modules use the callback paradigm. Within `async` functions, it is more convenient if functions return Promises instead so that the `await` keyword can be used. The `util` module provides a function, `util.promisify`, which generates a wrapper function for old-style callback-oriented functions so it instead returns a Promise.
