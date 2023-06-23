# Express Cheat Sheet

[![Express Logo](https://i.cloudup.com/zfY6lL7eFa-3000x3000.png)](http://expressjs.com/)

Express is a minimal and flexible Node.js web application framework that provides a robust set of features for web and mobile applications.

## Installation

To keep things organized, create a new folder for your project and navigate into it:

```bash
$ mkdir myapp
$ cd myapp
```

Initialize a package.json file

```bash
$ npm init -y
```

The `-y` flag will tell the command to use all default settings and create a `package.json` file for you. You can ignore the `-y` flag and see the questions npm will prompt you with.

Now you can install Express:

```bash
$ npm install express
```

To run a hello world node file, you need to use the command `node <filename>`. This is little tedious to type everytime. To make it easier, you can install `nodemon`.

[`Nodemon`](https://github.com/remy/nodemon) is a utility that will monitor for any changes in your source and automatically restart your server. Perfect for development. Install it using npm.

```bash
$ npm install -g nodemon
```

> Note: The `-g` flag will install the package globally. This means you can use it in any node project you create.

You can create a script in your `package.json` file to run your node file using nodemon. Open your `package.json` file and add the following line to the `scripts` object.

```json
"scripts": {
  "devStart": "nodemon index.js"
}
```

To run your node file, you can now use the command `npm run devStart`.

## Hello World

Create a file named `server.js` and add the following code to it:

```js
const express = require("express");
const app = express();

app.get("/", (req, res) => {
  res.send("Hello World!");
});

app.listen(3000, () => console.log("Server running on port 3000"));
```

Run the server using the command `npm run devStart` and navigate to `http://localhost:3000` in your browser. You should see the text `Hello World!` displayed on the page.

A more common then using `send()` is to use `json()` (i.e. you are building an api). This will send a JSON response back to the client.

```js
app.get("/", (req, res) => {
  res.json({ message: "Hello World!" });
});
```

You may want to send a status code back to the client. You can do this by chaining the `status()` method to the `send()` method.

```js
app.get("/", (req, res) => {
  res.status(500).send("Hello World!");
});
```

Your website may seems to work file but if inspect the console, you will see a 500 error.

You may want to send a user a file to download. You can do this by using the `download()` method.

```js
app.get("/", (req, res) => {
  res.download("./downloads/sample.pdf");
});
```

Another thing you may want to di is to render a view. You can do this by using the `render()` method.

A view engine may be used to render the view.

Since express is most used to build APIs, it will not be covered in this cheat sheet. but you can search for `express view engine` to learn more about rendering views instead of sending JSON responses.

## Routing

Routing refers to how an application’s endpoints (URIs) respond to client requests. You define routing using methods of the Express app object that correspond to HTTP methods; for example, `app.get()` to handle GET requests and `app.post` to handle POST requests.

keeping adding routes to the `server.js` file is not a great idea since it will get messy. You can create a folder called `routes` and create a file called `users.js` in it. Add the following code to it:

```js
const express = require("express");
const router = express.Router();

router.get("/", (req, res) => {
  res.json({ message: "Users Page" });
});

router.get("/new", (req, res) => {
  res.json({ message: "Create a New User" });
});

module.exports = router;
```

Now you can import this file in your `server.js` file and use it as a middleware.

```js
const express = require("express");
const app = express();

const usersRouter = require("./routes/users");

app.get("/", (req, res) => {
  res.json({ message: "Home Page" });
});

app.use("/users", usersRouter);

app.listen(3000, () => console.log("Server running on port 3000"));
```

Now if you navigate to `http://localhost:3000/users`, you should see the text `Users Page` displayed on the page.

## Url Parameters

You can pass parameters in the url. For example, you can pass the user id in the url.

```js
router.get("/:id", (req, res) => {
  res.json({ message: `User ${req.params.id}` });
});
```

Note how we access the parameter using `req.params.id`. You can access the parameters using `req.params.<parameter_name>`.

Take care how you order your routes. If you have a route `/users/new`, it will match the route `/users/:id` since `new` is a valid id. To avoid this, you should place the `/users/new` route above the `/users/:id` route.

With the same url pattern, you may write the logic for more or two HTTP verbs, instead of writing the same url pattern again and again, you can chain the HTTP verbs to the same url pattern.

```js
router
  .route("/:id")
  .get((req, res) => {
    res.json({ message: `User ${req.params.id}` });
  })
  .put((req, res) => {
    res.json({ message: `Update User ${req.params.id}` });
  })
  .delete((req, res) => {
    res.json({ message: `Delete User ${req.params.id}` });
  });
```

## Middleware

Middleware functions are functions that have access to the request object (req), the response object (res), and the next middleware function in the application’s request-response cycle. The next middleware function is commonly denoted by a variable named next.

Middleware functions can perform the following tasks:

- Execute any code.
- Make changes to the request and the response objects.
- End the request-response cycle.
- Call the next middleware function in the stack.

Let's create two middleware functions. The first one is instead of repeating the logic behind retrieving a user in each http method, we can create a middleware function that will do that for us using `router.param()`.

`router.param()` is used to run a function when a specific parameter is present in the url. In this case, we want to run a function when the `id` parameter is present in the url.

```js
router.param("id", (req, res, next, id) => {
  let user = users.find((user) => user.id === id); // simple array, in real life, you will use a database
  console.log(`User ${id} was found`);
  next();
});
```

The second middleware function `logger` will log the url and the method used to access the url.

```js
const logger = (req, res, next) => {
  console.log(`${req.protocol}://${req.get("host")}${req.originalUrl}`);
  next();
};
```

Now you can use the `logger` middleware function in your `server.js` file.

```js
const express = require("express");
const app = express();

const logger = require("./middleware/logger");

const usersRouter = require("./routes/users");

app.use(logger);

app.get("/", (req, res) => {
  res.json({ message: "Home Page" });
});

app.use("/users", usersRouter);

app.listen(3000, () => console.log("Server running on port 3000"));
```

## Access body of a request

To access the body of a request, you can do it via `req.body`. But you need to tell express to parse the body of the request. You can do this by using the `express.json()` middleware function.

```js
app.use(express.json());
```

Now you can access the body of the request.

```js
app.post("/", (req, res) => {
  console.log(req.body);
  res.json({ message: "User Created" });
});
```

## Access query parameters

To access the query parameters, you can use `req.query`.

```js
app.get("/", (req, res) => {
  console.log(req.query);
  res.json({ message: "Home Page" });
});
```

In your browser, navigate to `http://localhost:3000/?name=John&age=30`. You should see the following output in the console:

```js
{ name: 'John', age: '30' }
```
