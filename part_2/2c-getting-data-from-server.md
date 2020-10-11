# 2c. Getting data from server

[Back to index](../README.md)

## `axios` and `json-server`

```shell
npm install axios --save
npm install json-server --save-dev
```

### Example

```js
import axios from "axios";

axios
  .get("http://localhost:3001/notes")
  .then((response) => {
    console.log(response.data)
  });
```

## Promise

- Promise: an object that represents an asynchronous operation

### Three States

1. Pending
2. Fulfilled (or Resolved)
3. Rejected
