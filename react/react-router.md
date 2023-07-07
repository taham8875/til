# React Router

![react-router](assets/react-router.png)

## Installation

There are two versions of React Router:

- `react-router-dom` for web apps
- `react-router-native` for react-native apps

```bash
$ npm install react-router-dom
```

## Usage

To start using React Router, we need to wrap our application in a `BrowserRouter` component.

```tsx
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App";
import { BrowserRouter } from "react-router-dom";

ReactDOM.createRoot(document.getElementById("root")!).render(
  <React.StrictMode>
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </React.StrictMode>
);
```

There are different types of routers available:

- `BrowserRouter`: for web apps.
- `HashRouter`: store the current location in the hash portion of the URL `http://example.com/#/about` instead of the pathname `http://example.com/about`.
- `MemoryRouter`: doesn't read or write to the address bar. Useful in tests and non-browser environments like React Native.
- `StaticRouter`: for server-side rendering, tells react to render a specific location.
- `NativeRouter`: for react-native apps.

99% of the time, you'll use `BrowserRouter`.

## Route

The `Route` component is used to define a route. It has two required props:

- `path`: the path of the route
- `component`: the component to render when the path matches

Note that `Route` components must be wrapped in a `Routes` component.

```tsx
import { Routes, Route, Link } from "react-router-dom";

function App() {
  return (
    <>
      <nav>
        <ul>
          <li>
            <Link to="/">Home</Link>
          </li>
          <li>
            <Link to="about">About</Link>
          </li>
          <li>
            <Link to="contact-us">Contact Us</Link>
          </li>
          <li>
            <Link to="register">Register</Link>
          </li>
        </ul>
      </nav>
      <Routes>
        <Route path="/" element={<h1>Home</h1>} />
        <Route path="/books" element={<h1>Books</h1>} />
        <Route path="/books/:id" element={<h1>Book id</h1>} />
        <Route path="/books/new" element={<h1>New Books</h1>} />
        <Route path="/about" element={<h1>About</h1>} />
        <Route path="/contact-us" element={<h1>contact-us</h1>} />
        <Route path="/register" element={<h1>register</h1>} />
        <Route path="*" element={<h1>Not Found</h1>} />
      </Routes>
    </>
  );
}

export default App;
```

_Note_ that the `*` path is a catch-all route, it will match any path that hasn't been matched yet. (like a 404 page)

Note there are many routers starting with `/books`. We can use the `Router` component to render nested routes.

```tsx
<Router path="/books" element={<BookLayout/>} />
  <Route index element={<h1>Books</h1>} />
  <Route path="/:id" element={<h1>Book id</h1>} />
  <Route path="/new" element={<h1>New Books</h1>} />
</Router>
```

`index` is a special prop that tells the route to match the path exactly.

Let's create a `BookLayout` component that will render the `Book` component and a sidebar.

```tsx
const BookLayout = () => {
  return (
    <div>
      <div>
        <h1>Books</h1>
        <Link to="/books/new">New Book</Link>
      </div>
      <div>
        <Outlet />
      </div>
    </div>
  );
};
```

The `Outlet` component is used to render nested routes.

`Outlet` can has a `context` prop that can be used to pass data to the nested routes.

```tsx
const BookLayout = () => {
  return (
    <div>
      <div>
        <h1>Books</h1>
        <Link to="/books/new">New Book</Link>
      </div>
      <div>
        <Outlet context={{ books: ["clean code", "clean architecture "] }} />
      </div>
    </div>
  );
};

const Book = () => {
  const { books } = useOutletContext();
  return (
    <div>
      {books.map((book) => (
        <div>{book}</div>
      ))}
    </div>
  );
};
```

We can extract the url parameters from the path using the `useParams` hook.

```tsx
import React from "react";
import { Route, Routes, useParams } from "react-router-dom";
import Home from "./Home";
import About from "./About";
import User from "./User";

const App = () => {
  return (
    <Routes>
      <Route path="/" component={Home} />
      <Route path="/about" component={About} />
      <Route path="/user/:id" component={User} />
    </Routes>
  );
};

const User = () => {
  const { id } = useParams();
  return <div>User {id}</div>;
};
```

If you want to hardcode the location (you want it to render a specific route), you can use the `location` prop.

```tsx
function App() {
  return (
    <Routes location="/books">
      <Route path="/" element={<Home />} />
      <Route path="/about" element={<About />} />
      <Route path="/contact-us" element={<ContactUs />} />
      <Route path="/register" element={<Register />} />
      <Route path="*" element={<NotFound />} />
    </Routes>
  );
}
```

## Link

The `Link` component is used to create links to other routes. It has one required prop:

- `to`: the path to link to
- `replace`: if true, the current entry in the history stack will be replaced by the new one instead of adding a new one. (clicking the back button will not go back to the previous page, useful for login pages)

```tsx
import React from "react";
import { Link } from "react-router-dom";

const Home = () => {
  return (
    <div>
      <h1>Home</h1>
      <Link to="/about">About</Link>
      <Link to="/contact">Contact</Link>
      <Link to="/settings">Settings</Link>
    </div>
  );
};
```

## NavLink

The `NavLink` component is used to create links to other routes. It is similar to the `Link` component, but it has some extra props to style the link when it is active. It has one required prop:

- `to`: the path to link to
- `className`: the class to apply to the link, it works like normal `className` prop on any component, but you can also pass it a function to customize the classNames applied based on the active and pending state of the link.

```tsx
<NavLink
  to="/messages"
  className={({ isActive, isPending }) =>
    isPending ? "pending" : isActive ? "active" : ""
  }
>
  Messages
</NavLink>
```

- `style`: the style to apply to the link, it works like normal `style` prop on any component, but you can also pass it a function to customize the styles applied based on the active and pending state of the link.

```tsx
<NavLink
  to="/messages"
  style={({ isActive, isPending }) =>
    isPending
      ? { color: "gray" }
      : isActive
      ? { color: "red" }
      : { color: "black" }
  }
>
  Messages
</NavLink>
```

By default, an active class is added to a `<NavLink>` component when it is active so you can use CSS to style it.

```tsx
import React from "react";
import { NavLink } from "react-router-dom";

const Home = () => {
  return (
    <div>
      <h1>Home</h1>
      <NavLink to="/about" activeClassName="active">
        About
      </NavLink>
      <NavLink to="/contact" activeClassName="active">
        Contact
      </NavLink>
      <NavLink to="/settings" activeClassName="active">
        Settings
      </NavLink>
    </div>
  );
};
```

## Navigate

The `Navigate` component is used to navigate to another route.

- `to`: the path to navigate to

This may be used to redirect the user to the home page if he entered a wrong path.

```tsx
import React from "react";
import { Navigate } from "react-router-dom";

export function NotFound() {
  return <Navigate to="/" />;
}
```

But what if we don't want to navigate based on a component, but based on a condition?

## useNavigate

The `useNavigate` hook is used to navigate to another route.

```tsx
import React from "react";
import { useNavigate } from "react-router-dom";

export function NotFound() {
  const navigate = useNavigate();
  return <button onClick={() => navigate("/")}>Go Home</button>;
}
```

You can also instead of navigating to a hardcoded path, you can navigate to a previous path. (like hitting the back button)

```tsx
navigate(-1);
```

## Search Params

The `useSearchParams` hook is used to get the search params from the url.

```tsx
import React from "react";
import { useSearchParams } from "react-router-dom";

export function Search() {
  const [searchParams, setSearchParams] = useSearchParams();
  return (
    <div>
      <input
        type="text"
        value={searchParams.get("query") || ""}
        onChange={(e) => setSearchParams({ query: e.target.value })}
      />
    </div>
  );
}
```

## useLocation

The `useLocation` hook is used to get the current location.

```tsx
import React from "react";
import { useLocation } from "react-router-dom";

export function Search() {
  const location = useLocation();
  console.log(location);
  return <div>{location.pathname}</div>;
}
```

This will console log the current location object.

```tsx
{
  pathname: "/books",
  search: "?query=react",
  hash: "#the-hash",
  state: { fromDashboard: true },
  key: "tgll3z4w"
}
```

The location object has the following properties:

- `pathname`: the path of the current location
- `search`: the search params of the current location
- `hash`: the hash of the current location (the part after the #)
- `state`: the state of the current location, to share data between routes use the `state` prop on the `Link` or `Navlink` component.
- `key`: unique key for the current location used by react router

This can be useful if you'd like to perform some side effect whenever the current location changes.
