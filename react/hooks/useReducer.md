# useReducer

`useReducer` is a react hook that let you extract the state logic from a component to a reducer function.

Note that because it is a hook, you should only call it at the top level of your component, not inside loops, conditions, or nested functions.

When the state changes, react request a re-render of the component, if the nextState is equal to the current state (using `Object.is`), react will skip the re-render for performance reasons.

## Usage

```jsx
const [state, dispatch] = useReducer(reducer, initialState, init?)
```

## Example

```jsx
import { useReducer } from 'react'

function reducer(state, action) {
    // ...
}

function MyComponent() {
    const [state, dispatch] = useReducer(reducer, { age: 42})
    // ...
}
```

## Parameters

- `reducer`: A function that takes the current state and an action, and returns the next state. It should be pure and describe how the state is updated.
- `initialState`: The initial state of the reducer.
- `init`: An optional function that returns the initial state. This is useful for when the initial state is expensive to compute.

## Return value

An array with the current state and a dispatch function the lets you update the state to a different value.

## `dispatch` function

The dispatch function lets you update the state by calling it with an action. The action is an object that describes what happened. It should have a `type` property that is a string. You can also add other properties to the action object to pass additional data.

```jsx
function handleButtonClick() {
    dispatch({ type: 'increment' })
}

function handleColorChange() {
    dispatch({ type: 'change-color', color: 'blue' })
}
```

React will update the state to the value returned by the reducer function.

## `reducer` function

The reducer function takes the current state and an action, and returns the next state. It should be pure and describe how the state is updated.

```jsx
function reducer(state, action) {
    switch (action.type) {
        case 'increment':
            return { ...state, count: state.count + 1 }
        case 'change-color':
            return { ...state, color: action.color }
        default:
            throw new Error()
    }
}
```

Note that the state is read only, so don't modify any objects or arrays.

```jsx
// Don't do this
function reducer(state, action) {
    state.count += 1
    return state
}
// Do this
function reducer(state, action) {
    return { ...state, count: state.count + 1 }
}
```

> To increase the readability of the reducer function, you can use the `switch` statement instead of `if` statements.

# Using TypeScript

You can use TypeScript with `useReducer` by providing the type of the state and the action.

```tsx
import { useReducer } from 'react'

type State = { count: number }

type CounterAction =
    | { type: 'increment' }
    | { type: 'decrement' }
    | { type: 'set-count'; count: State['count'] }
    | { type: 'reset' }


function reducer(state: State, action: CounterAction): State {
    // ...
}
```
