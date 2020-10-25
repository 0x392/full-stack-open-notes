# 4c. User administration

[Back to index](../README.md)

## `bcrypt`

Installation:

```shell
npm install bcrypt --save
```

Usage:

```js
const hash = await bcrypt.hash(secret, saltRounds);
```

## `populate` Method of Mongoose

Mongoose accomplishes `JOIN` (supported by relational database) with the `populate` method

Example:

```js
// Select `content` and `date` fields
const fields = { content: 1, date: 1 };
const users = await User.find({}).populate("notes", fields);
```

The syntax `someDocument.populate("foo")` means that replacing the id (or an array of ids) in the `foo` field into the corresponding documents in the collection which is the `ref` option defined in the schema

Example:

`controllers/blogs.js`

```js
Blog.find({}).populate("user");
```

`models/blog.js`

```js
const blogSchema = mongoose.Schema({
  user: {
    type: mongoose.Schema.Types.ObjectId,
    ref: "User",
  },
});
```

`models/user.js`

```js
module.exports = mongoose.model("User", userSchema);
```

Note:

- The `"user"` (function argument) in `controllers/blogs.js` should match the `user` (object property) in `models/blog.js`
- The `"User"` (value of the `ref` property) in `models/blog.js` should match the `"User"` (function argument) in `models/user.js`
