# 3a. Node.js and Express

[Back to index](../README.md)

## Express

```shell
npm install express --save
```

### Middleware

- Middleware is a function used for handling `request` and `response` objects
- When there are more than one middleware, they'll be executed one by one, in the order that they're taken into use
- Have to be taken into use before routes
  - There are also situations where middleware functions are defined after routes

Example:

```js
const requestLogger = (req, res, next) => {
  console.log("Method:", request.method);
  console.log("Path:  ", request.path);
  console.log("Body:  ", request.body);
  console.log("---");
  // Yield control to the next middleware
  next();
};

const myLogger = (req, res, next) => {
  console.log("my logger");
  next();
};

const unknownEndpoint = (req, res, next) => {
  console.log("unknown endpoint");
  next();
};

const errorHandler = (err, req, res, next) => {
  console.log(err.message);

  if (err.name === "CastError")
    return res.status(400).send({ error: "malformed ID" });

  // Pass the error forward to default Express
  // error handler
  next(err);
};
```

```js
app.use(requestLogger);
app.use(myLogger);

app.get("/", (req, res, next) => {
  /* .. */
});

app.get("/example/next", (req, res, next) => {
  // If `next()` is called without a parameter,
  // then the execution will simply move onto the
  // next route or middleware
  next();
});

app.get("/example/error", (req, res, next) => {
  // If `next()` is called without a parameter,
  // then the execution will continue to the error
  // handler middleware
  somePromise.then(() => console.log("success")).catch((error) => next(error));
});

app.use(unknownEndpoint);
app.use(errorHandler);
```

## `nodemon`

```shell
npm install nodemon --save-dev
```

## REST

- Representational State Transfer
- An architecture style for building scalable web apps
- Every resource has an associated URL

## HTTP Request Types

Two properties of request types: safety and idempotence

### Safety

- GET and HEAD should be safe
- The executing request should not cause any side effects in the server
- Side effects: actions other than retrieval
  - Example: changing the state of database

### Idempotence

- All HTTP requests except POST should be idempotence
- If a request has side-effects, then the result should be same regardless of how many times the request is sent

Note: "safety" and "idempotence" are just recommendations in the HTTP standard; nothing can guarantee

<!-->
