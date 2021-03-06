# Fullstack Open Notes

## Part 0: Fundamentals of web apps

- 0b. [Fundamentals of web apps](part_0/0b-fundamentals-of-web-apps.md)
  - Traditional web applications
  - Single page applications (SPA)

## Part 2: Communicating with server

- 2c. [Getting data from server](part_2/2c-getting-data-from-server.md)
  - `axios` and `json-server`
  - Promise
- 2d. [Altering data in server](part_2/2d-altering-data-in-server.md)
  - Route

## Part 3: Programming a server with Node.JS and Express

- 3a. [Node.js and Express](part_3/3a-node.js-and-express.md)
  - Express, middleware
  - `nodemon`
  - REST
  - HTTP request types
- 3b. [Deploying app to internet](part_3/3b-deploying-app-to-internet.md)
  - CORS
  - `cors` middleware, `static` middleware
  - Proxy (`create-react-app`)
- 3c. [Saving data to MongoDB](part_3/3c-saving-data-to-mongodb.md)
  - Chrome dev tools
  - MongoDB, Mongoose
  - Define environment variable, `dotenv`
- 3d. [Validation and ESLint](part_3/3d-validation-and-eslint.md)
  - Validation in Mongoose
  - ESLint

## Part 4: Testing Express servers, user administration

- 4a. [Structure of backend application, introduction to testing](part_4/4a-structure-of-backend-application-introduction-to-testing.md)
  - Structure of backend application, extract logging
  - Automated testing with Jest
- 4b. [Testing the backend](part_4/4b-testing-the-backend.md)
  - Tests for backend
  - Test environment, database for testing
  - Testing API with `supertest`
  - The `async`, `await` syntax
    - Error handling, `express-async-errors`
  - `Promise.all(..)`
- 4c. [User administration](part_4/4c-user-administration.md)
  - `bcrypt`
  - `populate` method of Mongoose
- 4d. [Token authentication](part_4/4d-token-authentication.md)
  - JSON web token, login logic, error handling

## Part 5: Testing React apps

- 5a. [Login in frontend](part_5/5a-login-in-frontend.md)
  - Local storage
- 5b. [props.children and proptypes](part_5/5b-props-children-and–proptypes.md)
  - `ref`
  - proptypes
- 5c. [Testing React apps](part_5/5c-testing-react-apps.md)
  - `react-testing-library`
- 5d. [End-to-end testing](part_5/5d-end-to-end-testing.md)
  - End-to-end testing
  - Cypress

## Part 6: State management with Redux

- 6a. [Flux architecture and Redux](part_6/6a-flux-architecture-and-redux.md)
  - Flux architecture
  - Redux library
  - `deep-freeze`
  - Forwarding Redux store to various components
- 6b. [Many reducers](part_6/6b-many-reducers.md)
  - Combined reducers
  - Redux DevTools
- 6c. [Communicating with server in a Redux application](part_6/6c-communicating-with-server-in-a-redux-application.md)
  - `redux-thunk`
- 6d. [connect](part_6/6d-connect.md)
  - `connect` function
  - Presentational and container components
  - Redux-like state management

## Part 7: React router, custom hooks, styling apps with CSS and webpack

- 7a. [React router](part_7/7a-react-router.md)
  - React router
- 7b. [Custom hooks](part_7/7b-custom-hooks.md)
  - Examples of Hooks
  - Hooks rules
  - Custom hooks
  - More about hooks
- 7c. [More about styles](part_7/7c-more-about-style.md)
  - `react-bootstrap`
  - Material-UI
  - Styled components
- 7d. [Webpack](part_7/7d-webpack.md)
  - Webpack
  - Transpilation and polyfill
- 7e. [Class components, miscellaneous](part_7/7e-class-components-miscellaneous.md)
  - Class components
  - Miscellaneous

## Part 8: GraphQL

- 8a. GraphQL server
- 8b. React and GraphQL
- 8c. Database and user administration
- 8d. Login and updating the cache
- 8e. Fragments and subscriptions

## Part 9: Typescript

- 9a. Background and introduction
- 9b. First steps with Typescript
- 9c. Typing the express app
- 9d. React with types

## Part 10: React Native

- 10a. Introduction to React Native
- 10b. React Native basics
- 10c. Communicating with server
- 10d. Testing and extending our application
