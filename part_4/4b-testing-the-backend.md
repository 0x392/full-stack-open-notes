# 4b. Testing the backend

[Back to index](../README.md)

## Tests for Backend

- Unit tests
- Mocking the database
  - Example: `mongo-mock`
- Integration test
  - Multiple components of the system are being tested as a group
  - Example: testing REST API, database included

## Test Environment

`package.json`:

```json
{
  "scripts": {
    "start": "NODE_ENV=production node index.js",
    "dev": "NODE_ENV=development nodemon index.js",
    "test": "NODE_ENV=test jest --verbose --runInBand"
  }
}
```

- Node defines the execution mode of the application with `NODE_ENV` environment variable
- It's common to production, development and testing modes
- `cross-env` is used for fixing the issue of Windows
  - Installation: `npm install cross-env --save-dev`

### Database for Testing

- Creating a separate test database in MongoDB Atlas
  - Not an optimal solution in situations where there're many people developing the same application
- Better solution: using database that is installed and running in local machine
- Optimal solution: having every test execution use its own separate database
  - Running Mongo in-memory
  - Using Docker containers

## Testing API with `supertest`

Installation:

```shell
npm install supertest --save-dev
```

Example:

```js
const mongoose = require("mongoose");
const supertest = require("supertest");
const app = require("../app");

// 1. Wraps the Express application with the
//    `supertest` function making it into a
//    `superagent` object
// 2. Tests can use the object to make HTTP
//    requests to the backend
const api = supertest(app);

test("notes are returned as json", async () => {
  await api
    .get("/api/notes")
    .expect(200)
    .expect("Content-Type", /application\/json/);
});

test("there are two notes", async () => {
  const response = await api.get("/api/notes");

  // 1. Execution gets here only after the HTTP
  //    request is complete
  // 2. The result of HTTP request is saved in
  //    variable response
  expect(response.body).toHaveLength(2);
});

// Close the database connection once all the tests
// have finished running
afterAll(() => {
  mongoose.connection.close();
});
```

### Initializing the Database before Tests

Use the `beforeEach(..)` function offered by Jest:

```js
const Note = require("models/note");

const initialNotes = [
  {
    content: "content 1",
    date: new Date(),
    important: true,
  },
  {
    content: "content 2",
    date: new Date(),
    important: false,
  },
];

beforeEach(async () => {
  await Note.deleteMany({});

  let noteObject;

  noteObject = new Note(initialNotes[0]);
  await noteObject.save();

  noteObject = new Note(initialNotes[1]);
  await noteObject.save();
});
```

## The `async`, `await` Syntax

- Introduced in ES7
- Makes it possible to use **asynchronous** **functions** that return a promise in a way that makes the code look synchronous

Example 1:

```js
// Use a promise with a chaining `then(..)`
Note.find({}).then((notes) => {
  console.log(notes);
});

// Use async function instead
const notes = await Note.find({});
console.log(notes);
```

Example 2:

```js
// Use a promise with a chaining `then(..)`
Note.find({})
  .then((notes) => {
    return notes[0].remove();
  })
  .then((response) => {
    console.log("the first note is removed");
  });

// Use async function instead
const notes = await Note.find({});
const response = await notes[0].remove();

console.log("the first note is removed");
```

### Error Handling

Use `try...catch` mechanism

Example:

```js
router.post("/", async (req, res, next) => {
  // ...
  try {
    const savedData = await data.save();
    response.json(savedData);
  } catch (exception) {
    next(exception);
  }
});
```

By using `express-async-errors`, we can eliminate `try...catch` blocks completely

## `Promise.all(..)`

- It's used for transforming an array of promises into a single promise
  - Fulfilled once every promise in the array is resolved
- It executes the promises in parallel

Example:

```js
async function foo() {
  const promiseArray = noteObjects.map((note) => note.save());
  const results = await Promise.all(promiseArray);
}
```

If the promises need to be executed in particular order: using `for` loop

Example:

```js
async function foo() {
  for (let note of initialNotes) {
    let noteObject = new Note(note);
    await noteObject.save();
  }
}
```
