# Extracting State Logic into a Reducer

Component with many states updates spread across many event handlers can be overwhelming, for this reason you can extract the state logic into a reducer.

## The problem with `useState`

As your components grow, it can gets harder to see the different ways the states get updates.

for example:

```jsx
export default function TaskApp() {
  const [tasks, setTasks] = useState(initialTasks);

  function handleAddTask(text) {
    setTasks([
      ...tasks,
      {
        id: nextId++,
        text: text,
        done: false,
      },
    ]);
  }

  function handleChangeTask(task) {
    setTasks(
      tasks.map((t) => {
        if (t.id === task.id) {
          return task;
        } else {
          return t;
        }
      })
    );
  }

  function handleDeleteTask(taskId) {
    setTasks(tasks.filter((t) => t.id !== taskId));
  }
```

Reducers moves the logic into an easy to find place and makes it easier to read and test.

**Migrate from `useState` to `useReducer`**

1. Move from setting state to dispatching actions.
2. Write a reducer function
3. Use the reducer in your component.

### 1. Move from setting state to dispatching actions

So instead of telling react "what to do", you tell it "what the user did"

```jsx
function handleAddTask(text) {
    dispatch({
    type: "ADD_TASK",
    payload: {
        id: nextId++,
        text: text,
        done: false,
    },
    });
}

function handleChangeTask(task) {
    dispatch({
    type: "CHANGE_TASK",
    payload: task,
    });
}

function handleDeleteTask(taskId) {
    dispatch({
    type: "DELETE_TASK",
    payload: taskId,
    });
}
```

> The object you pass to `dispatch` is called an **action**. It should have a `type` property that is a string. You can also add other properties to the action object to pass additional data.

### 2. Write a reducer function

The reducer function takes the current state and an action, and returns the next state. It should be pure and describe how the state is updated.

```jsx
function reducer(state, action) {
  switch (action.type) {
    case "ADD_TASK": {
        return [...state, action.payload];
    }
    case "CHANGE_TASK": {
      return state.map((t) => {
        if (t.id === action.payload.id) {
          return action.payload;
        } else {
          return t;
        }
      });
    }
    case "DELETE_TASK": {
      return state.filter((t) => t.id !== action.payload);
    }
    default: {
      throw Error("Invalid action type" + action.type);
    }
  }
}
```

If you want, you can move the reducer function into a separate file.

## 3. Use the reducer in your component

Finally you can hook up the reducer you created to your component using `useReducer`.

```jsx
import { useReducer } from "react";

const [tasks, dispatch] = useReducer(reducer, initialTasks);
```

# `useState` vs `useReducer`

- **code size**: `useState` is simpler and requires less code, but `useReducer` can reduce the code size if you have a lot of state transitions.
- **Readability**: `useReducer` makes it easier to see how state is updated, because all updates are in one place. `useState` is easier to read when the state is simple.
- **Debugging**: `useReducer` makes it easier to debug because you can trace the state updates to the action that caused them. `useState` is harder to debug because the state updates are spread out across many event handlers.
- **Testing**: `useReducer` makes it easier to test because you can test the reducer function in isolation. `useState` is harder to test because the state updates are spread out across many event handlers.

# Writing well reducers

- Reducers must be pure functions. Same input always returns the same output.
- Reducers must not mutate the state. Always return a new object.
- Each action describes a single user interaction even if it triggers multiple state updates. For example, if you have a form with reset button the resets 5 fields, you should dispatch 1 action that resets all 5 fields `reset_form`, not 5 actions that reset 1 field each `reset_field`.
