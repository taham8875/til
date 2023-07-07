# Redux

Redux is a predictable state container for JavaScript apps. It helps you write applications that behave consistently, run in different environments (client, server, and native), and are easy to test. On top of that, it provides a great developer experience, such as live code editing combined with a time traveling debugger.

## Integrating Redux with a UI

Redux is a standalone state management library, which can be used with any UI layer or framework such as React, Angular, Vue, Ember, and vanilla JS. Although Redux and React are commonly used together, they are independent of each other.

Using Redux with any UI layer requires the same consistent set of steps:

1. Create a Redux store
2. Subscribe to updates
3. Inside the subscription callback:
   - Get the current store state
   - Extract the data needed by this piece of UI
   - Update the UI with the data
4. If necessary, render the UI with initial state
5. Respond to UI inputs by dispatching Redux actions

## Getting Started

### Installation

You can create a new project using the `create-react-app` command line tool, I prefere to use `vite` for this:

```bash
$ npm init vite@latest
```

Choose `React` and `TypeScript` as template.

Then install `@redux/toolkit` and `react-redux`:

```bash
$ npm install @reduxjs/toolkit react-redux
```

## Getting Started

inside `src` folder create a new folder called `app` and inside it create a file called `store.ts`:

Redux store is a plain JavaScript object that holds the application's state (app container). There should only be a single store in your app. To create it, pass your root reducing function to `createStore`.

_Note_ : It is recommended to install the Redux DevTools browser extension, which makes debugging much easier.

```ts
import { configureStore } from "@reduxjs/toolkit";

export const store = configureStore({
  reducer: {
    // here we will be adding reducers
  },
  devTools: process.env.NODE_ENV !== "production",
});
```

Then back to `index.tsx` file, import `Provider` from `react-redux` and wrap the `App` component with it:

```tsx
import React from "react";
import ReactDOM from "react-dom/client";
import { Provider } from "react-redux";
import { store } from "./app/store";

ReactDOM.createRoot(document.getElementById("root")!).render(
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>
);
```

Provider makes the Redux store available to the rest of your app.

## Slice

A slice is a collection of reducer logic and actions for a single feature of your app. A slice contains the initial state and all the logic to handle changes for a specific feature.

Create a new folder called `features` inside `app` folder and inside it create a file called `counterSlice.ts`:

```ts
import { createSlice } from "@reduxjs/toolkit";

interface CounterState {
  count: number;
}

const initialState: CounterState = {
  count: 0,
};

export const counterSlice = createSlice({
  name: "counter", // name of the slice
  initialState, // initial state
  reducers: {
    // actions associated with the slice
    increment: (state) => {
      state.count += 1;
    },
    decrement: (state) => {
      state.count -= 1;
    },
  },
});

export const { increment, decrement } = counterSlice.actions;

export default counterSlice.reducer;
```

Now we need to add the slice to the store, go back to `store.ts` file and import the slice reducer:

```ts
import { configureStore } from "@reduxjs/toolkit";
import counterReducer from "../features/counterSlice";

export const store = configureStore({
  reducer: {
    counter: counterReducer, // add the slice reducer
  },
});

// Infer the `RootState` and `AppDispatch` types from the store itself
export type RootState = ReturnType<typeof store.getState>;

// Inferred type: {counter: CounterState}
export type AppDispatch = typeof store.dispatch;
```

Now the counter slice is added to the store, and can be seen by the entire app.

## Defined Types Hooks

While it's possible to import the `RootState` and `AppDispatch` types into each component, it's better to create typed versions of the `useDispatch` and `useSelector` hooks for usage in your application. It saves you from having to type `RootState` and `AppDispatch` in every component file.

Create a file called `hooks.ts` inside `app` folder:

```ts
import { TypedUseSelectorHook, useDispatch, useSelector } from "react-redux";
import type { RootState, AppDispatch } from "./store";

// Use throughout your app instead of plain `useDispatch` and `useSelector`
export const useAppDispatch: () => (AppDispatch = useDispatch);
export const useAppSelector: TypedUseSelectorHook<RootState> = useSelector;
```

Let's create a component to use the counter slice, create a file called `Counter.tsx` inside `features` folder:

```tsx
import { useAppSelector, useAppDispatch } from "../../app/hooks";
import { decrement, increment } from "./counterSlice";

function Counter() {
  const count = useAppSelector((state) => state.counter.count);
  const dispatch = useAppDispatch();

  return (
    <div>
      <button onClick={() => dispatch(increment())}>+</button>
      <span>{count}</span>
      <button onClick={() => dispatch(decrement())}>-</button>
    </div>
  );
}

export default Counter;
```

As seen above, we used two custom hooks from `react-redux`:

- `useAppSelector`: allows you to extract data from the Redux store state, using a selector function, and subscribe to updates to that data.
- `useAppDispatch`: returns a reference to the dispatch function from the Redux store. This lets us dispatch actions to the store.

Now we can use the `Counter` component in `App` component:

```tsx
import Counter from "./features/Counter";

function App() {
  return <Counter />;
}
```

Run `npm run dev` and you will see the counter component.

Congratulations! You've successfully integrated Redux with React.

### Add more actions

You can add more actions to the slice, for example:

- `reset`: set the count to 0, you will need to add a button to the component to dispatch this action.
- `incrementByAmount`: increment the count by a specific amount, you will need to add an input to the component to dispatch this action and a button to submit the input value.
