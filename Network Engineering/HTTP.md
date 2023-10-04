# HTTP

## What is HTTP?

HTTP stands for HyperText Transfer Protocol. It is a protocol that allows the fetching of resources, such as HTML documents. It is the foundation of any data exchange on the Web and it is a client-server protocol, which means requests are initiated by the recipient, usually the Web browser. A complete document is reconstructed from the different sub-documents fetched, for instance text, layout description, images, videos, scripts, and more.

HTTP is stateless: there is no link between two requests being successively carried out on the same connection. Each request independently carries all the information necessary to understand it, for instance authorization, request context, and so on. The server therefore does not need to keep any information on the client and can provide responses accordingly, without any additional information being required.

## What is HTTPS?

HTTPS stands for HyperText Transfer Protocol Secure. It is the secure version of HTTP, the protocol over which data is sent between your browser and the website that you are connected to. The 'S' at the end of HTTPS stands for 'Secure'. It means all communications between your browser and the website are encrypted by Transport Layer Security (TLS) or Secure Sockets Layer (SSL). HTTPS is often used to protect highly confidential online transactions like online banking and online shopping order forms.

A lot of websites force HTTPS to ensure that all communications between their servers and your browser are encrypted. _try to navigate to http://www.google.com and see how it redirect you to https://www.google.com_. You can tell if a website uses HTTPS by looking at the URL in your browser's address bar. If the URL begins with "https://" instead of "http://", the site uses HTTPS.

## HTTP Methods

HTTP defines a set of request methods to indicate the desired action to be performed for a given resource. Although they can also be nouns, these request methods are sometimes referred to as HTTP verbs. Each of them implements a different semantic, but some common features are shared by a group of them: e.g. a request method can be safe, idempotent, or cacheable.

Examples :

- `GET`: The GET method requests a representation of the specified resource. Requests using GET should only retrieve data.

- `POST`: The POST method is used to submit an entity to the specified resource, often causing a change in state or side effects on the server.

- `PUT`: The PUT method replaces all current representations of the target resource with the request payload.

- `DELETE`: The DELETE method deletes the specified resource.

## HTTP Headers

HTTP headers let the client and the server pass additional information with an HTTP request or response.

For example:

- `User-Agent`: This header is sent by the client (e.g., a web browser) and provides information about the client's user agent or application. It helps servers understand the type and version of the client.

- `Content-Type`: In an HTTP response, this header specifies the type of data being sent. For example, it might indicate that the content is in HTML, JSON, XML, or some other format.

- `Content-Length`: This header indicates the size of the content being sent in the response, helping the client know how much data to expect.

- `Location`: Often used in HTTP responses with status codes like 3xx (redirects), this header tells the client where to find the requested resource.

- `Accept`: In an HTTP request, the client can use this header to specify the types of content it can handle, helping the server decide how to format the response.

- `Authorization`: This header is used to send authentication credentials to the server, typically in the form of a token.

- `Cache-Control`: Both in requests and responses, this header defines caching directives, instructing how the content should be cached by the client or intermediary caches.

- `Referer`: Sent by the client, it indicates the URL from which the current request originated. This can be useful for tracking the source of traffic.

- `Server`: In an HTTP response, this header reveals information about the server software and version running on the server.

- `Set-Cookie`: Sent in an HTTP response, this header is used to set cookies on the client's side for session management and tracking.

- `User-Agent`: Similar to the User-Agent header in requests, the server can also include a User-Agent header in its responses to provide information about the server software and version.

## HTTP Status Codes

HTTP response status codes indicate whether a specific HTTP request has been successfully completed or not. Responses are grouped in five classes:

- `Informational responses (1xx)`: These responses indicate that the request was received and understood. They are informational and are usually not used.

- `Successful responses (2xx)`: These responses indicate that the request was successfully received, understood, and accepted.

- `Redirects (3xx)`: These responses indicate that the client must take some additional action in order to complete their request.

- `Client errors (4xx)`: These responses indicate that there was an error in the request, most commonly a syntax error.

- `Server errors (5xx)`: These responses indicate that there was an error on the server side, preventing the request from being completed.

Common HTTP status codes include:

- `200 - OK`
- `201 - Created`
- `301 - Moved Permanently`
- `304 - Not Modified`
- `400 - Bad Request`
- `401 - Unauthorized`
- `403 - Forbidden`
- `404 - Not Found`
- `500 - Internal Server Error`
