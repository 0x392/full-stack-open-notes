# 6c. Communicating with server in a Redux application

[Back to index](../README.md)

- It would be better to abstract the communication away from the components
  - The components only have to call the appropriate action creator

## `redux-thunk`

- `redux-thunk` is a Redux middleware
  - Must be initialized along with the initialization of the store
- `redux-thunk` enables us to create asynchronous action creators

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
import { useDispatch } from "react-redux";
import { initializeNotes } from "./reducers/noteReducer";

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

// `initializeNote` is an action creator that
// returns an async function:
//   1. First an asynchronous operation is executed
//   2. Then, the action changing the state of the
//      store is dispatched
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
