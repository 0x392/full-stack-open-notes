# 5d. End-to-end testing

[Back to index](../README.md)

## End-to-end Testing

End-to-end (E2E) tests:

- Test the system as a whole
- Pros
  - Testing the system through the same interface as the real users use
- Cons
  - Configuration is more challenging
  - Quite slow
  - Can be flaky

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
    "cypress:open": "cypress open"
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
- When both frontend and backend are running, we can start Cypress wth `npm run cypress:open`

### Test Files

Test files are placed in the `cypress/integration` directory

Cypress commands:

- `cy.visit(..)`
- `cy.contains(..)`
  - `cy.contains(..).click()`
- `cy.get(..)`
  - `cy.get("input:first").type("username")`
  - `cy.get("input:last").type("password")`

### Plugins for ESLint

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
