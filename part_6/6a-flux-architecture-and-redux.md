# 6a. Flux architecture and Redux

[Back to index](../README.md)

## Flux Architecture

Flux offers a standard way for:

- How and where the state is kept (store)
- How the state is modified (dispatch actions)

The state in Flux architecture:

- It's separated completely from the React components into its own "stores"
- It's not changed directly
  - Using different "actions" to change

Action -> Dispatcher -> Store -> View

## Redux Library

Installation:

```shell
npm install redux
```

### Components of Redux

- State
- Store
- Actions
  - Action creators
- Reducers

#### Commonly Used Functions of Redux

- `createStore`
- `store.dispatch({ type: ... })`: dispatches an action to the store
- `store.getState()`
- `store.subscribe(() => { ... })`: creates callback functions the store calls when its state is changed

### State and Store

- The state is stored into a Javascript object in the "store"
- The state is changed with actions

### Actions

Actions are objects with at least a property `type`

Example:

```js
simple_action = { type: "INCREMENT" };

action_with_data = {
  type: "NEW_NOTE",
  data: {
    id: 1,
    content: "I'll see you at the beginning, friend.",
  },
};
```

#### Action Creators

Action creators: functions that create actions

### Reducers

- Handle actions: a reducer defines the impact of the action to the state
- A reducer is a function
  - Parameters: current state, action
  - Returns a new state

Note:

- Reducers are never supposed to be called directly from application code
- Reducers are only given as a parameter to the `createStore` function
- Reducers must be pure functions
  - Do not cause any side effects
  - Always return the same response when called with the same parameters
  - Don't use `stats.push(..)`; use `stats.concat(..)` instead
  - Using `deep-freeze` to ensure the reducer has been correctly defined as an immutable function
- A reducer state must be composed of immutable objects
  - If there is a change in the state, the old object is replaced with a new, changed object

### Example: Counter App

```js
import { createStore } from "redux";

const counterReducer = (state = 0, action) => {
  switch (action.type) {
    case "INCREMENT":
      return state + 1;
    case "DECREMENT":
      return state - 1;
    case "ZERO":
      return 0;
    default:
      return state;
  }
};

const store = createStore(counterReducer);
```

## `deep-freeze`

Used to ensure the reducer has been correctly defined as an immutable function

Installation:

```shell
npm install deep-freeze --save-dev
```

Example:

```js
import deepFreeze from "deep-freeze";
import appReducer from "./appReducer";

test("some test", () => {
  const state = {
    /* ... */
  };
  const action = {
    /* ... */
  };

  deepFreeze(state);
  const newState = appReducer(state, action);
  expect(/* ... */);
});
```

## Forwarding Redux Store to Various Components

When a component is composed of many smaller components, there must be a way for all of the components to access the store

### `react-redux` Library

Using the hooks API of the `react-redux` library

Installation:

```shell
npm install react-redux
```

Example:

```js
// index.js
// 1. Import `Provider` component from the
//    `react-redux` library
// 2. Wrap the `App` component with a `Provider`
//    component (with `store` prop)
// 3. There is no need to use `store.subscribe(..)`
//    for re-rendering now
import React from "react";
import ReactDOM from "react-dom";
import { Provider } from "react-redux";
import store from "./store";
import App from "./App";

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById("root")
);
```

```js
// App.js
import React from "react";
import SomeComponent from "./components/SomeComponent";
import AnotherComponent from "./components/AnotherComponent";

const App = () => {
  return (
    <div>
      <SomeComponent />
      <AnotherComponent />
    </div>
  );
};

export default App;
```

```js
// AnotherComponent.js
import React from "react";
import { useSelector, useDispatch } from "react-redux";

const AnotherComponent = () => {
  const note = useSelector((state) => state.note);
  const dispatch = useDispatch();

  const handleClick = (id) => {
    dispatch({ type: "LIKE", data: { id } });
  };

  return (
    <div>
      <div>{note.content}</div>
      <div>
        <button onClick={() => handleClick(note.id)}>Like</button>
      </div>
    </div>
  );
};

export default AnotherComponent;
```

## Presentational and Container Components

- Presentational components
- Container components
  - Contains some application logic (e.g. event handlers)
  - Coordinates the configuration of presentational components
