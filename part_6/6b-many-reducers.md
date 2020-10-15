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

Note: every action gets handled in evry part of combined reducer

## Redux DevTools

Chrome extension: [here](https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd)

Library:

```shell
npm install redux-devtools-extension --save-dev
```

Change the definition of the store:

```js
import { composeWithDevTools } from "redux-devtools-extension";

const store = createStore(reducer, composeWithDevTools());
```
