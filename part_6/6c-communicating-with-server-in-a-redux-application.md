# 6c. Communicating with server in a redux application

[Back to index](../README.md)

It would be better to abstract the communication away from the components

## `redux-thunk`

- `redux-thunk` is a redux-middleware
  - Must be initialized along with the initialization of the store
- `redux-thunk` is able to create asynchronous actions

Installation:

```shell
npm install redux-thunk --save
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
import React, { useEffect } from "react";
import { initializeNotes } from "./reducers/noteReducer";
import { useDispatch } from "react-redux";

const App = () => {
  const dispatch = useDispatch();

  useEffect(() => {
    dispatch(initializeNotes());
  }, [dispatch]);

  /* return ... */
};

export default App;
```

```js
// reducers/noteReducer.js
import noteService from "../services/notes";

// Returns an async function: first an asynchronous
// operation is executed; then the action changing
// the state of the store is dispatched
export const initializeNotes = () => {
  return async (dispatch) => {
    const notes = await noteService.getAll();
    dispatch({
      type: "INIT_NOTES",
      data: notes,
    });
  };
};

export default noteReducer;
```
