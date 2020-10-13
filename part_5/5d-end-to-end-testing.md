# 5d. End-to-end testing

[Back to index](../README.md)

## End-to-end Testing

End-to-end (E2E) tests:

- Test the system as a whole
- Pros:
  - Testing the system through the same interface as the real users use
- Cons:
  - Configuration is more challenging
  - Quite slow
  - Can be flaky (sometimes pass, sometimes fail)

End-to-end testing of a web app:

- Browser and a testing library (e.g. Selenium)
  - Another browser option: headless browsers

## E2E Library: Cypress

Cypress tests are run completely within the browser (different than most E2E testing libraries)

Installation (to frontend):

```shell
npm install cypress --save-dev
```

Adding an npm-script:

```json
// `package.json` in frontend
{
  "scripts": {
    // using GUI
    "cypress:open": "cypress open",

    // runs in the terminal
    "test:e2e": "cypress run"
  }
}
```

```json
// `package.json` in backend
{
  "scripts": {
    "start:test": "cross-env NODE_ENV=test node index.js"
  }
}
```

### Run Cypress

- The tests required the Cypress test system to be running
- When both frontend and backend (`npm run start:test`) are running, we can start Cypress wth `npm run cypress:open`

### Test Files

Test files are placed in the `cypress/integration` directory

Cypress commands:

- `cy.visit(..)`
- `cy.contains(..)`
  - `cy.contains(..).click()`
  - Or using `cy.get(..).should(..)`
- `cy.get(..)` searches for the whole page; examples:
  - `cy.get("input:first").type("username")`
  - `cy.get("input:last").type("password")`
  - `cy.get(".error").contain(..)`
- `cy.request(..)`, example:
  - `cy.request("POST", "/api/users", user)`

#### Examples of `cy.should(..)`

```js
cy.get(".error").should("contain", "wrong credentials");
// Color should be given as rgb
cy.get(".error").should("have.css", "color", "rgb(255, 0, 0)");
cy.get(".error").should("have.css", "border-style", "solid");

cy.get("html").should("not.contain", "Signed in as Chimmy");
```

Chaining:

```js
cy.get(".error")
  .should("contain", "wrong credentials")
  .and("have.css", "color", "rgb(255, 0, 0)")
  .and("have.css", "border-style", "solid");
```

Check [here](https://docs.cypress.io/guides/references/assertions.html#Common-Assertions) to see more common assertions

### Bypassing the UI

For logging, instead of using the form in the `beforeEach` block, it's recommended to bypass the UI and do a HTTP request to the backend

Example:

```js
beforeEach(function () {
  cy.request("POST", "/api/login", {
    username: "username",
    password: "password",
  }).then((response) => {
    localStorage.setItem("app-signed-user", JSON.stringify(response.body));
    cy.visit("http://localhost:3000");
  });
});
```

### Custom Commands

Custom commands are declared in `cypress/support/commands.js` directory

Example:

```js
Cypress.Commands.add("login", ({ username, password }) => {
  cy.request("POST", "/api/login", {
    username,
    password,
  }).then(({ body }) => {
    localStorage.setItem("app-signed-user", JSON.stringify(body));
    cy.visit("http://localhost:3000");
  });
});
```

Then the command `cy.login({"some_username", "some_password"})` can be used in testing files

### `as`

Example:

```js
cy.contains(".some-class").as("theButton");
cy.get("@theButton").click();
```

### Debugging

- Cypress commands are like promises
- Access the returned values with the `.then(..)` method

Example:

```js
it("example", function () {
  cy.get("button").then((buttons) => {
    console.log("number of buttons", buttons.length);
    cy.wrap(buttons[0]).click();
  });
});
```

### Others

#### Arrow Functions

When writing tests, it's recommended not to use arrow functions; they might cause some issues in certain situations

#### Videos

Can be found in `cypress/videos` directory

#### Plugins for ESLint

```shell
npm install eslint-plugin-cypress --save-dev
```

`.eslintrc.js`:

```js
module.exports = {
  env: {
    // ...
    "cypress/globals": true,
  },
  plugins: [
    // ...
    "cypress",
  ],
};
```
