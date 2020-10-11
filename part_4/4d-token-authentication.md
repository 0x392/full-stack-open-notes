# 4d. Token authentication

[Back to index](../README.md)

![Sequence diagram](https://fullstackopen.com/static/8b2839fe97680c325df6647121af66c3/5a190/16e.png)

## JSON Web Token

Installation:

```shell
npm install jsonwebtoken --save
```

### Implement Login Logic

```js
router.post("/", async (request, response) => {
  const user = await User.findOne({ username: request.body.username });
  const isPasswordCorrect =
    user === null
      ? false
      : await bcrypt.compare(body.password, user.passwordHash);

  if (!isPasswordCorrect) {
    // 401 unauthorized
    return response.status(401).json({
      error: "invalid username or password",
    });
  }

  const userForToken = {
    username: user.username,
    id: user._id,
  };
  const token = jwt.sign(userForToken, process.env.SECRET);

  response
    .status(200)
    .send({ token, username: user.username, name: user.name });
});
```
