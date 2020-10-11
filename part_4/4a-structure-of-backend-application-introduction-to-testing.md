# 4a. Structure of backend application, introduction to testing

[Back to index](../README.md)

## Structure of Backend Application

See [here](https://github.com/0x392/nodejs-backend-sample)

### Extract Logging

- Extract logging into its own module (`utils/logger.js`)
- If we wanted to send the logs to an external logging service, we would only have to make change in one place

```js
const info = (...params) => {
  console.log(...params);
};

const error = (...params) => {
  console.error(...params);
};

module.exports = { info, error };
```

## Automated Testing with Jest

### Install Jest

```shell
npm install --save-dev jest
```

### Set up `package.json`

```json
{
  "scripts": {
    "test": "jest --verbose --runInBand"
  },
  "jest": {
    "testEnvironment": "node"
  }
}
```

The `--runInBand` option prevents Jest from running tests in parallel

### Example of a Test File

Note that Jest expects the names of test files contain `.test`

```js
const palindrome = require("./palindrome");

// A describe block
describe("average", () => {
  // 1. Each `test` function defines an individual
  //    test case
  // 2. The first parameter: test description
  // 3. The second parameter: a function defines the
  //    functionality for the test case
  test("of many is calculated right", () => {
    // Execute the code to be tested, and then
    // verify the results with the `expect`
    // function
    expect(average([1, 2, 3, 4, 5, 6])).toBe(3.5);
  });

  test("of empty array is zero", () => {
    expect(average([])).toBe(0);
  });
});
```

### Run a Single Test

Possible solutions:

- Use `test.only(..)`
- `npm test -- -t "tests with a specific name"`
  - Refer the name of the test or the describe block
- `npm test -- tests/some_test.test.js`

### Matchers

- `toBe(..)`
- `toContain(..)`: it uses the `===` operator
- `toContainEqual(..)`
- `toEqual(..)`
- `toHaveLength(..)`
- `toMatch(..)`
- ...

### Describe Block

- Describe blocks can be used for grouping
- Describe blocks are necessary when we want to run some shared setup or teardown operations for a group of tests
