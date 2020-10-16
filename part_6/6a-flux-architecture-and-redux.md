# 6a. Flux architecture and Redux

[Back to index](../README.md)

## Flux Architecture

Flux offers a standard way for:

- How and where the state is kept
- How the state is modified

The state in Flux architecture:

- Separated completely from the React components into the "stores"
- Not changed directly
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
- Action
- Reducer

#### Commonly Used Functions

- `createStore`: creates the store
- `store.dispatch({ type: ... })`: dispatches an action to the store
- `store.getState()`
- `store.subscribe(() => { ... })`: creates callback functions the store calls when its state is changed

### State and Store

- The state is stored into a Javascript object in the "store"
- The state is changed with actions

### Actions

An action is an object with at least a property `type`

Examples:

```js
action = { type: "INCREMENT" };

action = {
  type: "NEW_NOTE",
  data: {
    id: 1,
    content: "I'll see you at the beginning, friend.",
  },
};
```

### Action Creators

Action creators: functions that create actions

### Reducers

- Handle actions: a reducer defines the impact of the action to the state
- A reducer is a function
  - Parameters: current state, action
  - Returns a new state
  - Never supposed to be called directly
    - Only given as a parameter to the `createStore` function
  - Must be pure functions
    - Do not cause any side effects
    - Always returns the same response with the same parameters
    - Don't use `stats.push(..)`; use `stats.concat(..)` instead
- A reducer state must be composed of immutable objects

### Example: Note App

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

## Forwarding Redux State to Various Components

### `react-redux` Library

Using the hooks API of the `react-redux` library

Installation:

```shell
npm install react-redux
```

Example:

```js
// index.js
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
import OtherComponent from "./components/OtherComponent";

const App = () => {
  return (
    <div>
      <SomeComponent />
      <OtherComponent />
    </div>
  );
};

export default App;
```

```js
// SomeComponent.js
import React from "react";
import { useDispatch, useSelector } from "react-redux";

const SomeComponent = () => {
  const note = useSelector((state) => state.note);
  const dispatch = useDispatch();

  const handleClick = (id) => {
    dispatch({ type: "LIKE", data: { id } });
  };

  return (
    <div>
      <div>{note}</div>
      <div>
        <button onClick={() => handleClick(note.id)}>Like</button>
      </div>
    </div>
  );
};

export default SomeComponent;
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

## Presentational and Container Components

- Presentational components
- Container components
  - Contains some application logic (e.g. event handlers)
  - Coordinates the configuration of presentational components
