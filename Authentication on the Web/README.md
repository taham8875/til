# Authentication on the Web

## Authentication vs Authorization

- _Authentication_: Who are you? (401 Unauthorized)
- _Authorization_: What are you allowed to do? (403 Forbidden)

## Stateful vs Stateless Authentication

- _Stateful_: Server keeps track of the user's state (session, cookies)
- _Stateless_: Server does not keep track of the user's state (JWT, OAuth)

## Session-based Authentication

- User logs in with username and password
- Server verifies credentials from the database
- Server creates a temporary session
- Server creates a cookie with the session ID
- User sends cookie with each request
- Server verifies session ID in cookie and grants access
- When user logs out, server destroys session and cookie

### Features

- Every user session is stored on the server (stateful)
  - Memory (less scalable)
  - Cache (e.g. Redis)
  - Database (e.g. PostgreSQL, MongoDB)
- Every user session has a unique session ID

  - Opaque reference to the session
    - No 3rd party can extract information from the session ID
    - Only the server can decode the session ID
  - Stored in a cookie
    - Signed with a secret key
    - Protected from tampering
    - Protected with flags

- Used in frameworks like Spring, Ruby on Rails. And scripting languages like PHP.

### Cookies

- Cookies are headers, just like `content-type`.
- Used in session management, tracking, personalization, etc.
- Consists of a name-value pair and optional attributes and flags.
- Set by server with `Set-Cookie` header.

Example:

```http
HTTP/1.1 200 OK
Content-type: text/html
Set-Cookie: SESS_ID=2m8vl4j4k1j3n4ko32v03mj054gv7rjh9bvd; Domain=example.com; Path=/; Expires=Wed, 13 Jan 2021 22:23:01 GMT;
```

```http

```

### Security

- Signed with a secret key to prevent tampering, if it is modified, it will break the signature and the server will notice it.
- If a 3rd party gets access to the cookie, they can log in as the user.

### Attributes

- `Domain` and `Path` specify the scope of the cookie.
- `Exipration` specifies when the cookie expires.
  - When omitted, the cookie is deleted when the browser is closed. (called a session cookie)

### Flags

- `HTTPOnly` prevents JavaScript from accessing the cookie.
- `Secure` only sends the cookie over encrypted HTTPS channels.
- `SameSite` can only be sent with requests originating from the same origin as the target domain. (i.e. co CORS sharing)

### CSRF

- Cross-Site Request Forgery
- Attacker tricks the user into performing an action on a website.
- Attacker sends a request to the website with the user's cookie.
- Website thinks the request is legitimate and performs the action.

## Token-based Authentication

### Flow

- User logs in with username and password
- Server generates a temporary token and embeds user information in it
- Server sends the token to the user (in body or header)
- User stores the token (e.g. in local storage or session storage)
- User sends the token with each request
- Server verifies the token and grants access
- When user logs out, token is deleted from the client storage

### Features

- Token are not stored on the server (stateless) (client-side only)
- Token are signed with a secret key to prevent tampering
- Token can be opaque or self-contained
  - self-contained tokens contain user information in its payload
  - It can reduce database lookups, but exposes user information to XSS attacks
- Typically sent in the `Authorization` header
- Used in Single Page Applications (SPAs) and mobile apps

## JWT

```http
HTTP/1.1 200 OK
Content-type: application/json
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJTdXBlckF1dGhlbnRpY2F0aW9uIiwic3ViIjoiMSIsImlhdCI6MTYwNjI4NjQwMCwiZXhwIjoxN
```

contains header (meta), payload (claims) and signature, delimited by dots `.`

### Security

- Signed with a secret key to prevent tampering
- Rarely encrypted, because web clients need to read token payload
- So no sensitive information should be stored in the token
- And tokens should be short-lived

### XSS

- Cross-Site Scripting
- Attacker injects malicious JavaScript code into a website, which is then executed by the user's browser.
- Attacker can steal user information, initiate AJAX requests, etc.
- Can be prevented by sanitizing user input and escaping output.

### Client-side Storage

JWT tokens are typically stored in `localStorage` or `sessionStorage`.

- `localStorage` stores data with no expiration date
- `sessionStorage` stores data for one session (data is lost when the browser tab is closed)

#### Local Storage

Browser key value store, simple javascript API.

**Pros**:

- Domain specific (every domain has its own local storage, other domains cannot access it)
- Max size is (5MB), bigger than cookies (4KB)

**Cons**:

- Plain text, no encryption
- Limited to strings, no objects (need to serialize/deserialize)
- Can't be used with web workers (only accessible from the main thread)
- Stored forever, unless manually deleted
- Accessible from JavaScript (XSS attacks)

**Best for:**

- Public, non-sensitive data

**Worst for:**

- Sensitive data (passwords, tokens, etc.)
- Non string data (objects, binary data, etc.)

## Session vs Token

### Session + Cookies

**Pros**:

- Opaque session ID, and carry no meaningful information
- Can be secured with flags (HTTPOnly, Secure, SameSite)
- HTTPOnly prevents XSS attacks
- Battle tested for 20+ years, in many languages and frameworks

**Cons**:

- Requires server-side storage
- Session auth must be secured against CSRF attacks
- Horizontal scaling is difficult (need to share sessions between servers)

### JWT

**Pros**:

- Server does not need to store sessions
- Horizontal scaling is easy (no shared state between servers, any server can verify the token)
- Frontend and backend are decoupled (no need to share sessions between frontend and backend), can be used with mobile apps and SPAs
- Operational even if cookies are disabled

**Cons**:

- Server still to maintain a blacklist of revoked tokens (for example, when the user changes his password)
  - Defeats the purpose of stateless authentication
- When scaling horizontally, the secret key must be shared between servers
- Data stores in the token is "cached" and can become stale (out of sync with the database)
- Exposing user information to XSS attacks, attacker can steal the token and access website resources on behalf of the user

## Options for Auth in SPAs / APIs

1. Sessions
2. Stateless JWT
3. Stateful JWT

### Stateless JWT

- User payload is embedded in the token
- Token is signed with a secret key and `base64url` encoded
- Token is sent in the `Authorization` header
- Stored in `localStorage` or `sessionStorage`
- Server can retrieve user information from the token
- No sessions stores server-side

### Stateful JWT

- Only user ref (e.g. user ID) is embedded in the token, and retrieved from the database
- Token is signed with a secret key and `base64url` encoded
- Sent as `HTTPOnly` cookie (`Set-Cookie` header)
- No user session is stored server-side

### Sessions

- Session are stored server-side and linked to a user by a session ID
- Session ID is signed and stored in a cookie
- Sent via `Set-Cookie` header
- Can be secured with flags (`HTTPOnly`, `Secure`, `SameSite`)
- Scoop can be limited with `Domain` and `Path`

## Conclusion

Sessions are (probably) the best option for web apps and websites.

### Why not JWT?

- Server state is required anyway (blacklist of revoked tokens)
- Sessions are easily extended or invalidated (in case of JWT we have to maintain the refresh token which is a little bit more complex)
- Data is secured in the server side (XSS attacks are more difficult)
- Data never becomes stale (always in sync with the database)
- Sessions are generally easier to setup and use
- Most applications don't require enterprise-level scalability