# 6b. Many reducers

[Back to index](../README.md)

## Combined Reducers

Example:

```js
import { createStore, combineReducers } from "redux";

const reducer = combineReducers({
  notes: noteReducer,
  filter: filterReducer,
});

const store = createStore(reducer);
```

Note: every action gets handled in every part of combined reducer

### Reading State

```js
import { useSelector } from "react-redux";

const notes = useSelector((state) => state.notes);
```

### Dispatching Actions

We don't need to specify which part of the state that we dispatch actions to; reducers will handle these actions and update the state accordingly

```js
import { useDispatch } from "react-redux";

const dispatch = useDispatch();
```

## Redux DevTools

Chrome extension: [here](https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd)

Library:

```shell
npm install redux-devtools-extension --save-dev
```

Change the definition of the store to get the library up and running:

```js
import { composeWithDevTools } from "redux-devtools-extension";

const store = createStore(reducer, composeWithDevTools());
```
