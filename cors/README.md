# CORS

CORS (Cross Origin Resource Sharing) is a mechanism that allows a web page to make AJAX requests to another domain. This is a security feature of browsers that prevents a malicious website from making requests to another website.

By default browsers supports same-origin policy, which means that a web page can only make AJAX requests to the same domain that the page was loaded from. For example, if you have a web page loaded from `http://example.com`, then it can only make AJAX requests to URLs on `http://example.com`. If you try to make an AJAX request to `http://example2.com`, then the browser will block the request.

To enable CORS, you need to add the following headers to the response of the API:

```
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: GET, POST, PUT, DELETE, OPTIONS
Access-Control-Allow-Headers: Content-Type, Authorization
```

Let's break down each of these headers:

- `Access-Control-Allow-Origin`: This header is used to control which domains can make cross-origin requests to the API. The value `*` means that any domain can make requests to the API. If you want to allow only a specific domain, then you can set the value to that domain. For example, `Access-Control-Allow-Origin: http://example.com` will allow only `http://example.com` to make requests to the API.

- `Access-Control-Allow-Methods`: This header is used to control which HTTP methods can be used to make requests to the API. The value `GET, POST, PUT, DELETE, OPTIONS` means that all these methods are allowed. If you want to allow only specific methods, then you can set the value to those methods. For example, `Access-Control-Allow-Methods: GET, POST` will allow only `GET` and `POST` methods.

- `Access-Control-Allow-Headers`: This header is used to control which HTTP headers can be used in the request. The value `Content-Type, Authorization` means that the `Content-Type` and `Authorization` headers are allowed. If you want to allow only specific headers, then you can set the value to those headers. For example, `Access-Control-Allow-Headers: Content-Type` will allow only `Content-Type` header.

# Preflight Requests

Preflight requests are used to check if the API supports CORS. The browser will send a preflight request before making the actual request. The preflight request will have the `OPTIONS` method and the `Access-Control-Request-Method` and `Access-Control-Request-Headers` headers. The server should respond with the `Access-Control-Allow-Methods` and `Access-Control-Allow-Headers` headers.

Preflight request are *not* sent for simple requests. A simple request is one that meets all the following conditions:

- The request method is `GET`, `HEAD`, or `POST`
- The `Content-Type` header is `application/x-www-form-urlencoded`, `multipart/form-data`, or `text/plain`

If the request is not a simple request, then the browser will send a preflight request. For example, if you make a `PUT` request with `Content-Type: application/json`, then the browser will send a preflight request.

# Configure CORS

To enable CORS in your API, you need to add the following code to your `server.js` file:

```js
app.use(function(req, res, next) {
  res.header("Access-Control-Allow-Origin", "*");
  res.header("Access-Control-Allow-Methods", "GET, POST, PUT, DELETE, OPTIONS");
  res.header("Access-Control-Allow-Headers", "Content-Type, Authorization");
  next();
});
```

For simplicity, `express` provides a middleware that can be used to enable CORS:

```js
var cors = require('cors');
app.use(cors({
    origin: '*',
    methods: 'GET, POST, PUT, DELETE, OPTIONS',
    allowedHeaders: 'Content-Type, Authorization'
}));
```
