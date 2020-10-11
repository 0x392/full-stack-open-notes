# 3c. Saving data to MongoDB

[Back to index](../README.md)

## Chrome Dev Tools

```shell
node --inspect index.js
```

## MongoDB

Types of databases:

- Relational databases
- Document databases
  - Schema-less: it's possible to store documents with completely different fields in the same collection

MongoDB is a document database

### MongoDB Structure

- Database: stores collections
- Collection: stores documents
- (BSON) document: a data record

#### BSON

- A binary representation of JSON documents
- `_id` field
  - Reserved as a primary key
  - Must be unique in the collection
  - Immutable

## Mongoose

### Specify the Database and Connect

The database is specified in the connection string uri

```js
const db = "note-app-test";
const url = `mongodb+srv://${username}:${password}@${server}/${db}?retryWrites=true&w=majority`;
console.log("Connecting to", url);

mongoose
  .connect(url, {
    useNewUrlParser: true,
    useUnifiedTopology: true,
    useFindAndModify: false,
    useCreateIndex: true,
  })
  .then((result) => {
    console.log("Connected to MongoDB");
  })
  .catch((error) => {
    console.log("Error connecting to MongoDB:", error.message);
  });
```

### Define `Schema` and `model`

```js
// The `Schema` defines the structure of records
// (document) to be stored
const noteSchema = new mongoose.Schema({
  content: String,
  date: Date,
  important: Boolean,
});

// 1. The `Schema` is then compiled into a `model`
// 2. The first parameter ("Note" here) of `model`
//    is the singular name of the model.
// 3. The name of the collection is the plural one
//    in lowercase (`notes` here`)
const Note = mongoose.model("Note", noteSchema);
```

### Create and Save a New Document

```js
const note = new Note({
  content: "Hello World.",
  date: new Date(),
  important: true,
});

note.save().then((result) => {
  console.log("note saved!");
  mongoose.connection.close();
});
```

### `Model.find(..)`

```js
// The parameter is an object expressing search
// conditions
Note.find({}).then((result) => {
  result.forEach((note) => {
    console.log(note);
  });
  mongoose.connection.close();
});
```

Restrict the search to include important notes only

```js
Note.find({ important: true }).then((result) => {
  // ...
});
```

Other methods:

- `Model.findById(..)`
- `Model.findByIdAndRemove(..)`
- `Model.findByIdAndUpdate(..)`

Example of `Model.findByIdAndUpdate(..)`

```js
const note = {
  content: body.content,
  important: body.important,
};

// 1. The method receives a regular JavaScript
//    object as its parameter; not a new note
//    object created with the `Note` model
// 2. By default, the event handler receives the
//    original document without modifications;
//    Adding the option `{ new: true }` to make it
//    called with the modified one instead
Note.findByIdAndUpdate(request.params.id, note, { new: true })
  .then((updatedNote) => {
    response.json(updatedNote);
  })
  .catch((error) => next(error));
```

### Format the Objects Returned by Mongoose

One way is to modify the `toJSON` method of the schema:

```js
personSchema.set("toJSON", {
  transform: (document, returnedObject) => {
    returnedObject.id = returnedObject._id.toString();
    // `_id` is in fact an object though it looks
    // like a string
    delete returnedObject._id;
    // `__v` is the mongo versioning field
    delete returnedObject.__v;
  },
});
```

- `response.JSON(obj)` (provided by Express) invokes `JSON.stringify(..)` method, which in turn invokes the `toJSON(..)` method of each element of `obj`
- While calling `toJSON(..)` method, the value (a function) of `transform` property will be invoked first (in the last few lines), and then take the`returnedObject` as the argument of `JSON.stringify(..)`

#### Call Stack

- `response.json(notes)`
  - `JSON.stringify(notes)`
    - `toJSON(..)`
      - `transform(..)`

### Virtuals

[Reference](https://mongoosejs.com/docs/guide.html)

Virtuals:

- Document properties
- Not persisted to MongoDB

#### Example: Virtual Property Getter

```js
personSchema.virtual("fullName").get(function () {
  return `${this.name.first} ${this.name.last}`;
});
```

## Environment Variable

### Define Environment Variable

#### Define in Command Line

```shell
MONGODB_URI="mongodb..." npm run dev
```

#### `dotenv` Library

Installation:

```shell
npm install dotenv --save
```

Create a `.env` file at the root of the project (Note that it should be gitignored right away)

```plaintext
MONGODB_URI="..."
PORT=3001
```

Usage

```js
require("dotenv").config();

const PORT = process.env.PORT;
```
