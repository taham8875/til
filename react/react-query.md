# React Query Cheat Sheet

React Query is a library for fetching data, managing, caching, syncing and updating server state in React applications.

React is unopinionated about how you fetch data, but as your app grows, it is usually a good idea to introduce a library like React Query to help you.

## Installation

```bash
$ npm install react-query
```

## Server setup

React Query is designed to work with any server implementation. You can use any server library you like, but for this example we will use [json-server](https://github.com/typicode/json-server) to create a fake REST API with all the HTTP verbs in less than 30 seconds.

install json-server:

```bash
$ npm install -g json-server
```

create new react app with typescript via vite:

```bash
$ npm init vite@latest
```

create a `db.json` file with some data:

```json
{
  "superheroes": [
    {
      "id": 1,
      "name": "Batman",
      "alterEgo": "Bruce Wayne"
    },
    {
      "id": 2,
      "name": "Superman",
      "alterEgo": "Clark Kent"
    },
    {
      "id": 3,
      "name": "Wonder Woman",
      "alterEgo": "Princess Diana"
    }
  ]
}
```

in the `package.json` file, add a new script to start the server:

```json
{
  "scripts": {
    // ...
    "serve-json": "json-server --watch db.json --port 3001"
  }
}
```

if you run `npm run serve-json`, the server will start and you will be able to access the data via `http://localhost:3001/superheroes`.

Let's make a refresh on how we traditionally fetch data from the server in React using `axios`, `useEffect` and `useState`:

```tsx
import { useState, useEffect } from "react";
import axios from "axios";

type SuperHero = {
  id: number;
  name: string;
  alterEgo: string;
};

type GetSuperHeroesResponse = {
  data: SuperHero[];
};

export const SuperHeroesPage = () => {
  const [isLoading, setIsLoading] = useState(true);
  const [superHeroes, setSuperHeroes] = useState([]);

  useEffect(() => {
    axios
      .get<GetSuperHeroesResponse>("http://localhost:3001/superheroes")
      .then((response) => {
        setSuperHeroes(response.data);
        setIsLoading(false);
      })
      .catch((error) => {
        console.log(error);
      });
  }, []);

  if (isLoading) {
    return <h2>Loading...</h2>;
  }

  return (
    <>
      <h2>Super Heroes Page</h2>
      <ul>
        {superHeroes.map((superHero) => (
          <li key={superHero["id"]}>
            {superHero["name"]} - {superHero["alterEgo"]}
          </li>
        ))}
      </ul>
    </>
  );
};
```

## Basic Usage

First you need to wrap your app with the `QueryClientProvider` component:

```tsx
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App";
import { QueryClient, QueryClientProvider } from "react-query";

const queryClient = new QueryClient();

ReactDOM.createRoot(document.getElementById("root")!).render(
  <React.StrictMode>
    <QueryClientProvider client={queryClient}>
      <App />
    </QueryClientProvider>
  </React.StrictMode>
);
```

Then you can use the `useQuery` hook to fetch data from the server:

```tsx
import { useQuery } from "react-query";
import axios from "axios";

export const RQSuperHeroesPage = () => {
  const { isLoading, data } = useQuery("super-heroes", () => {
    return axios.get("http://localhost:3001/superheroes");
  });

  if (isLoading) {
    return <h2>Loading...</h2>;
  }

  return (
    <>
      <h2>RQ Super Heroes Page</h2>
      <ul>
        {data?.data.map((superHero: any) => (
          <li key={superHero["id"]}>
            {superHero["name"]} - {superHero["alterEgo"]}
          </li>
        ))}
      </ul>
    </>
  );
};
```

## Handling Errors

```tsx
export const RQSuperHeroesPage = () => {
  // Notice how to we destruct the isError and error properties from the useQuery hook
  const { isLoading, data, isError, error } = useQuery("super-heroes", () => {
    // change the url to something that doesn't exist to see the error
    return axios.get("http://localhost:3001/some-random-url");
  });

  if (isLoading) {
    return <h2>Loading...</h2>;
  }

  // new
  if (isError) {
    return <h2>Error: {error.message}</h2>;
  }

  return (
    // ...
  );
};

```

## DevTools

React Query comes with a DevTools component that you can use to debug your queries. To use it, you need to add the `ReactQueryDevtools` component to your app:

```tsx
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App";
import { QueryClient, QueryClientProvider } from "react-query";
import { ReactQueryDevtools } from "react-query/devtools";

const queryClient = new QueryClient();

ReactDOM.createRoot(document.getElementById("root")!).render(
  <React.StrictMode>
    <QueryClientProvider client={queryClient}>
      <App />
      <ReactQueryDevtools initialIsOpen={false} position="bottom-right" />
    </QueryClientProvider>
  </React.StrictMode>
);
```

Open up the browser and you will see a floating icon on the bottom right of the screen. Click on it to open the DevTools.

## Query Caching

React Query caches the data by default. If you make the same query twice, the second time it will return the data from the cache.

Open your network tab and set the connection to fast 3G connection to slow down the request. Then hard refresh the page and you will see that the data is fetched from the server and the loading text is shown. Now refresh the page again and you will see that the data is fetched from the cache.

The data is fetched from the cache, after a time interval. By default, the time interval is 5 minutes, react query will refetch the data from the server and update it if the data has changed.

Another property to destruct from the `useQuery` hook is `isFetching`. If `isLoading` is set to false how we can track the loading state of the query? The answer is `isFetching`. `isFetching` is set to true when the query is fetching data from the server or the cache and it's set to false when the data is fetched.

`isLoading` is set to true only when the data is fetched from the server, if there are a cached data, `isLoading` is set to false before and after the data is fetched from the cache.

Try to destruct the `isFetching` property from the `useQuery` hook and log it to the console:

```tsx
const { isLoading, data, isError, error } = useQuery("super-heroes", () => {
  return axios.get("http://localhost:3001/superheroes");
});

console.log({ isLoading, isFetching });
```

You can change this interval by passing a `cacheTime` property to the `useQuery` hook:

```tsx
const fetchSuperHeroes = () => {
  return axios.get("http://localhost:3001/superheroes");
};

const { isLoading, data, isError, error } = useQuery({
    "super-heroes",
    fetchSuperHeroes,
    { cacheTime: 10_000 } // 10 milliseconds
});
```

`staleTime` is another properties that you can pass to the `useQuery` hook.

You may need to read [this](https://stackoverflow.com/questions/72828361/what-are-staletime-and-cachetime-in-react-query) for better understanding of the difference between `cacheTime` and `staleTime`.

`staleTime`: The duration until a query transitions from fresh to stale. As long as the query is fresh, data will always be read from the cache only - no network request will happen! If the query is stale (which per default is: instantly (0 seconds)), you will still get data from the cache, but a background refetch can happen under certain conditions.

`cacheTime`: The duration until inactive queries will be removed from the cache. This defaults to 5 minutes. Queries transition to the inactive state as soon as there are no observers registered, so when all components which use that query have unmounted.

## Fetch on Button Click

There are times when you want to fetch data on button click.

```tsx
import { useQuery } from "react-query";
import axios from "axios";

const fetchSuperHeroes = () => {
  return axios.get("http://localhost:3001/superheroes");
};

export const RQSuperHeroesPage = () => {
  const { isLoading, data, refetch } = useQuery(
    "super-heroes",
    fetchSuperHeroes,
    {
      enabled: false,
    }
  );

  if (isLoading) {
    return <h2>Loading...</h2>;
  }

  return (
    <>
      <h2>RQ Super Heroes Page</h2>
      <button onClick={refetch}>Refetch</button>
      <ul>
        {data?.data.map((superHero: any) => (
          <li key={superHero["id"]}>
            {superHero["name"]} - {superHero["alterEgo"]}
          </li>
        ))}
      </ul>
    </>
  );
};
```

## Success and Error Callbacks

You can pass a `onSuccess` or `onError` callback to the `useQuery` hook, which will be called when the query is successful or when the query fails.

```tsx
export const RQSuperHeroesPage = () => {
  const onSuccess = (data) => {
    console.log("Fetch Super Heroes Successfully", data);
  };

  const onError = (error) => {
    console.log("Error Fetching Super Heroes", error);
  };

  const { isLoading, data } = useQuery("super-heroes", fetchSuperHeroes, {
    onSuccess,
    onError,
  });

  // ...
};
```

Open up the console and you will see the success message, change the url to an invalid url and you will see the error message.

## Data Transformations

You can transform the data before it's returned from the `useQuery` hook, this is helpful since the data returned from the server may not be in the format that you want. (i.e. a big json object and you just want to extract only the hero name)

```tsx
import { useQuery } from "react-query";
import axios from "axios";

const fetchSuperHeroes = () => {
  return axios.get("http://localhost:3001/superheroes");
};

export const RQSuperHeroesPage = () => {
  const { isLoading, data } = useQuery("super-heroes", fetchSuperHeroes, {
    select: (data) => {
      const superHeroesNames = data.data.map((superHero) => superHero.name);
      return superHeroesNames;
    },
  });

  if (isLoading) {
    return <h2>Loading...</h2>;
  }

  return (
    <>
      <h2>RQ Super Heroes Page</h2>
      <ul>
        {/* before */}
        {/* {data?.data.map((superHero: any) => (
          <li key={superHero["id"]}>
            {superHero["name"]} - {superHero["alterEgo"]}
          </li>
        ))} */}
        {/* after */}
        {data?.map((heroName: any) => (
          <li key={heroName}>{heroName}</li>
        ))}
      </ul>
    </>
  );
};
```

## Custom Query Hook

After configuring the `useQuery` hook, you can create a custom hook to reuse this configuration instead of repeating it in every component.

create a new folder called `hooks` and create a new file called `useSuperHeroesData.ts`, move the logic from the `RQSuperHeroesPage` component to the `useSuperHeroesData` hook.

```tsx
import { useQuery } from "react-query";
import axios from "axios";

const fetchSuperHeroes = () => {
  return axios.get("http://localhost:3001/superheroes");
};

export const useSuperHeroesData = (onSuccess, onError) => {
  return useQuery("super-heroes", fetchSuperHeroes, {
    onSuccess,
    onError,
    select: (data) => {
      const superHeroesNames = data.data.map((superHero) => superHero.name);
      return superHeroesNames;
    },
  });
};
```

Now you can use the `useSuperHeroesData` hook in the `RQSuperHeroesPage` component.

```tsx
import { useSuperHeroesData } from "../hooks/useSuperHeroesData";
import { useSuperHeroesData } from "../hooks/useSuperHeroesData";

export const RQSuperHeroesPage = () => {
  const onSuccess = () => {
    console.log("onSuccess");
  };

  const onError = () => {
    console.log("onError");
  };

  const { isLoading, data } = useSuperHeroesData(onSuccess, onError);

  // ...
};
```

## Query By Id

You can query data by id, for example if have a products cards and when you click on a product card you want to fetch the product details.

Let's create a new component called `RQSuperHeroPage` and add a new route for it.

Add a new route for the `RQSuperHeroPage` component.

```tsx
function App() {
  // ...
  <Route path="/super-heroes/:id" component={RQSuperHeroPage} />;
  // ...
}
```

```tsx
import { useParams } from "react-router-dom";
import { useSuperHeroData } from "../hooks/useSuperHeroData";

function RQSuperHeroPage() {
  const { id } = useParams();
  console.log("heroId", id);
  const { isLoading, data } = useSuperHeroData(id);

  if (isLoading) {
    return <h2>Loading...</h2>;
  }

  return (
    <>
      <h2>RQ Super Hero Page</h2>
      <h3>
        {data?.data.name} - {data?.data.alterEgo}
      </h3>
    </>
  );
}

export default RQSuperHeroPage;
```

Note that we used a custom hook called `useSuperHeroData`, let's create this hook.

```tsx
import { useQuery } from "react-query";
import axios from "axios";

const fetchSuperHero = ({ queryKey }) => {
  const heroId = queryKey[1];
  return axios.get(`http://localhost:3001/superheroes/${heroId}`);
};

export const useSuperHeroData = (heroId) => {
  return useQuery(["super-hero", heroId], fetchSuperHero, {});
};
```

Let's change our heros list to be links to the hero page (`RQSuperHeroesPage` component).

```tsx
export const RQSuperHeroesPage = () => {
  // ...
  return (
    // ...
    {data?.data.map((superHero: any) => (
      <li key={superHero["id"]}>
        <Link to={`/rq-super-heroes/${superHero["id"]}`}>
          {superHero["name"]} - {superHero["alterEgo"]}
        </Link>
      </li>
    ))}
    // ...
  )
}
```

## Parallel Queries

Execute multiple queries in parallel is as simple as calling the `useQuery` hook multiple times.

lets create a new component called `RQParallelQueriesPage` and add a new route for it.

```tsx
import { useQuery } from "react-query";
import axios from "axios";

const fetchSuperHeroes = () => {
  return axios.get("http://localhost:3001/superheroes");
};

const fetchVillains = () => {
  return axios.get("http://localhost:3001/villains");
};

export const RQParallelQueriesPage = () => {
  const { isLoading: isLoadingHeroes, data: heroes } = useQuery(
    "super-heroes",
    fetchSuperHeroes
  );

  const { isLoading: isLoadingVillains, data: villains } = useQuery(
    "villains",
    fetchVillains
  );

  if (isLoadingHeroes || isLoadingVillains) {
    return <h2>Loading...</h2>;
  }

  return (
    <>
      <h2>RQ Parallel Queries Page</h2>
      <h3>Super Heroes</h3>
      <ul>
        {heroes?.data.map((superHero: any) => (
          <li key={superHero["id"]}>
            {superHero["name"]} - {superHero["alterEgo"]}
          </li>
        ))}
      </ul>
      <h3>Villains</h3>
      <ul>
        {villains?.data.map((villain: any) => (
          <li key={villain["id"]}>
            {villain["name"]} - {villain["alterEgo"]}
          </li>
        ))}
      </ul>
    </>
  );
};
```

Note how we used aliases to avoid naming conflicts between the two queries.

## Dynamic Parallel Queries

You can also execute multiple queries in parallel dynamically, for example if you have a list of products and you want to fetch the details of each product.

Let's create a new component called `RQDynamicParallelQueriesPage` and add a new route for it.

```tsx
import { useQueries } from "react-query";
import axios from "axios";

const fetchSuperHero = ({ queryKey }) => {
  const heroId = queryKey[1];
  return axios.get(`http://localhost:3001/superheroes/${heroId}`);
};

export const RQDynamicParallelQueriesPage = () => {
  const heroesIds = [1, 2, 3, 4, 5];
  const heroesQueries = useQueries(
    heroesIds.map((heroId) => {
      return {
        queryKey: ["super-hero", heroId],
        queryFn: () => fetchSuperHero(heroId),
      };
    })
  );

  return (
    <>
      <h2>RQ Dynamic Parallel Queries Page</h2>
      <ul>
        {heroesQueries.map((heroQuery: any) => (
          <li key={heroQuery.data?.data.id}>
            {heroQuery.data?.data.name} - {heroQuery.data?.data.alterEgo}
          </li>
        ))}
      </ul>
    </>
  );
};
```

## Dependent Queries

You can also execute queries that depend on the result of another query. (execute sequentially, not in parallel).

Let's create a new component called `RQDependentQueriesPage` and add a new route for it.

```tsx
import { useQuery } from "react-query";
import axios from "axios";

const fetchSuperHero = ({ queryKey }) => {
  const heroId = queryKey[1];
  return axios.get(`http://localhost:3001/superheroes/${heroId}`);
};

const fetchSuperHeroBestFriend = ({ queryKey }) => {
  const friendId = queryKey[1];
  return axios.get(`http://localhost:3001/best-friend/${friendId}`);
};

export const RQDependentQueriesPage = () => {
  const { data: superHero } = useQuery(["super-hero", 1], fetchSuperHero);

  const friendId = superHero?.data?.bestFriendId;
  const { data: bestFriend } = useQuery(
    ["best-friend", friendId],
    fetchSuperHeroBestFriend,
    {
      enabled: !!friendId,
    }
  );

  console.log("superHero", superHero);
  console.log("bestFriend", bestFriend);

  return <div> RQ Dependent Queries Page </div>;
};
```

## Initial Queries Data

You can pass initial data to the `useQuery` hook, this is useful when you want to show some data while the query is loading.

We need to access the `queryClient` instance (provided to the entire app component) to pass the initial data.

Let's modify our `useSuperHeroData` hook to accept initial data.

```tsx
import { useQuery, useQueryClient } from "react-query";
import axios from "axios";

const fetchSuperHero = ({ queryKey }) => {
  const heroId = queryKey[1];
  return axios.get(`http://localhost:3001/superheroes/${heroId}`);
};

export const useSuperHeroData = (heroId, initialData) => {
  const queryClient = useQueryClient();
  return useQuery(["super-hero", heroId], fetchSuperHero, {
    initialData: () => {
      return (
        queryClient
          .getQueryData(["super-hero"])
          ?.data.find((hero: any) => hero.id === heroId) || undefined
      );
    },
  });
};
```

## Pagination

To work on pagination we need to add a new array of objects to our json-server (`db.json`)

```json
"colors" : [
    {
      "id": 1,
      "name": "red"
    },
    {
      "id": 2,
      "name": "blue"
    },
    {
      "id": 3,
      "name": "green"
    },
    {
      "id": 4,
      "name": "yellow"
    },
    {
      "id": 5,
      "name": "black"
    },
    {
      "id": 6,
      "name": "white"
    }
  ]
```

Let's create a new component called `RQPaginationPage` and add a new route for it.

```tsx
import { useQuery } from "react-query";
import axios from "axios";

const fetchColors = () => {
  return axios.get(`http://localhost:3001/colors`);
};

export const RQPaginationPage = () => {
  const { isLoading, isError, data, error } = useQuery("colors", fetchColors);

  if (isLoading) {
    return <h2>Loading...</h2>;
  }

  if (isError) {
    return <h2>Error: {error.message}</h2>;
  }

  return (
    <>
      <h2>RQ Pagination Page</h2>
      <ul>
        {data?.data.map((color: any) => (
          <li key={color["id"]}>{color["name"]}</li>
        ))}
      </ul>
    </>
  );
};
```

Note that json-server supports pagination out of the box, you can pass the `page` and `limit` query params to the request.

`https://localhost:3001/colors?_limit=3&_page=1`

- `_limit` is the number of items per page
- `_page` is the page number

So let's use these query params to implement pagination.

```tsx
import { useState } from "react";
import { useQuery } from "react-query";
import axios from "axios";

const fetchColors = ({ queryKey }) => {
  const pageNumber = queryKey[1];
  // limit is hardcoded to 1 for this example
  return axios.get(`http://localhost:3001/colors?_limit=1&_page=${pageNumber}`);
};

export const RQPaginationPage = () => {
  const [pageNumber, setPageNumber] = useState(1); // keep track of the current page number

  const { isLoading, isError, data, error } = useQuery(
    ["colors", pageNumber],
    fetchColors,
    {
      keepPreviousData: true, // keep previous data while fetching the next page
    }
  );

  if (isLoading) {
    return <h2>Loading...</h2>;
  }

  if (isError) {
    return <h2>Error: {error.message}</h2>;
  }

  return (
    <>
      <h2>RQ Pagination Page</h2>
      <ul>
        {data?.data.map((color: any) => (
          <li key={color["id"]}>{color["name"]}</li>
        ))}
      </ul>
      {/* 
      Add the previous and next page buttons
      
      disable the previous page button if we are on the first page */}
      <button
        onClick={() => {
          setPageNumber((prevPageNumber) => prevPageNumber - 1);
        }}
        disabled={pageNumber === 1}
      >
        Previous Page
      </button>
      <button
        onClick={() => {
          setPageNumber((prevPageNumber) => prevPageNumber + 1);
        }}
      >
        Next Page
      </button>

      <p>Page Number: {pageNumber}</p>
    </>
  );
};
```

## Infinite Queries

To work on infinite queries we need to add a new array of objects to our json-server (`db.json`)

```json

```

Let's create a new component called `RQInfiniteQueriesPage` and add a new route for it.

```tsx
import { useQuery, useInfiniteQuery } from "react-query"; // import useInfiniteQuery
import axios from "axios";
import { Fragment } from "react";

// useInfiniteQuery provides the fetchColors function with a pageParam argument
const fetchColors = ({ pageParam = 1 }) => {
  return axios.get(`http://localhost:3001/colors?_limit=2&_page=${pageParam}`);
};

export const RQInfiniteQueriesPage = () => {
  const {
    isLoading,
    isError,
    data,
    error,
    fetchNextPage, // fetchNextPage function
    hasNextPage, // boolean to check if there is a next page
    isFetched,
    isFetchingNextPage,
  } = useInfiniteQuery(["colors"], fetchColors, {
    getNextPageParam: (_lastPage, pages) => {
      // this should not be hardcoded, but json server doesn't support the functionality to know the total number of pages
      if (pages.length < 10) {
        return pages.length + 1;
      } else {
        return undefined;
      }
    },
  });

  if (isLoading) {
    return <h2>Loading...</h2>;
  }

  if (isError) {
    return <h2>Error: {error.message}</h2>;
  }

  return (
    <>
      <h2>RQ Infinite Queries Page</h2>
      <ul>
        {data?.pages.map((group, i) => (
          <Fragment key={i}>
            {group.data.map((color: any) => (
              <li key={color["id"]}>{color["name"]}</li>
            ))}
          </Fragment>
        ))}
      </ul>

      <button
        onClick={() => {
          fetchNextPage();
        }}
        disabled={!hasNextPage}
      >
        {isFetchingNextPage
          ? "Loading more..."
          : hasNextPage
          ? "Load More"
          : "Nothing more to load"}
      </button>
      <p>Is Fetched: {isFetched.toString()}</p>
    </>
  );
};
```

## Mutations

Let's learn how to make a post request using react query

let's create a new component called `RQMutationsPage` , in this component we will create a form to add a new super hero to our json-server.

```tsx
import { useState } from "react";
import { useMutation } from "react-query";
import axios from "axios";

const addSuperHero = (superHero) => {
  return axios.post(`http://localhost:3001/superheroes`, superHero);
};

export const RQMutationsPage = () => {
  const [name, setName] = useState("");
  const [alterEgo, setAlterEgo] = useState("");

  const { mutate, isLoading, isError, error, isSuccess } =
    useMutation(addSuperHero);

  const handleSubmit = (e) => {
    e.preventDefault();
    mutate({
      id: crypto.randomUUID(),
      name,
      alterEgo,
    });
  };

  if (isLoading) {
    return <h2>Loading...</h2>;
  }

  if (isError) {
    return <h2>Error: {error.message}</h2>;
  }

  if (isSuccess) {
    return <h2>Super Hero Added Successfully</h2>;
  }

  return (
    <>
      <h2>RQ Mutations Page</h2>
      <form onSubmit={handleSubmit}>
        <input
          type="text"
          placeholder="Name"
          value={name}
          onChange={(e) => setName(e.target.value)}
        />
        <input
          type="text"
          placeholder="Super Power"
          value={alterEgo}
          onChange={(e) => setAlterEgo(e.target.value)}
        />
        <button type="submit">Add Super Hero</button>
      </form>
    </>
  );
};
```

## Query Invalidation

Imagine if the form of creating a new hero and hero list are in the same page, we want to invalidate the query of the hero list after adding a new hero.

```tsx
import { useMutation, useQueryClient } from "react-query";

// ...

export const RQMutationsPage = () => {
  const queryClient = useQueryClient();
  // ...

  const { mutate, isLoading, isError, error, isSuccess } = useMutation(
    addSuperHero,
    {
      onSuccess: () => {
        queryClient.invalidateQueries("super-heroes");
      },
    }
  );

  // ...
};
```

## Handling Mutation Responses

Now when we add a new hero the browser creates `POST` request followed by a `GET` request to fetch the new data, it is a common practice to for the server to return the new data after a successful `POST` request, so we don't have to make a new `GET` request.

So how to get the response of the mutation

```tsx
// ...

const { mutate, isLoading, isError, error, isSuccess } = useMutation(
  addSuperHero,
  {
    onSuccess: (data) => {
      // queryClient.invalidateQueries("super-heroes");
      queryClient.setQueryData("super-heroes", (oldQueryData: any) => {
        return {
          ...oldQueryData,
          data: [...oldQueryData.data, data.data],
        };
      });
    },
  }
);

// ...
```

A little bit more code but saves you from making an additional request.
