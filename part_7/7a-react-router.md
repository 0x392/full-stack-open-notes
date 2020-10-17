# 7a. React router

[Back to index](../README.md)

Routing: conditional rendering of components based on the url of the browser

## React Router

- An excellent solution for managing navigation in a React app
- React Router divides the application into different views that are shown based on the URL of the browser

Installation:

```shell
npm install react-router-dom --save
```

### Structure

```js
import { BrowserRouter as Router, Switch, Route, Link } from "react-router-dom";

<Router>
  <Link to="/notes">Notes</Link>
  <Link to="/users">Users</Link>

  <Switch>
    <Route path="/notes">
      <Notes />
    </Route>
    <Route path="/users">
      <Users />
    </Route>
    <Route path="/">
      <Home />
    </Route>
  </Switch>
</Router>;
```

### Components

- `Router`
  - Placing components as children of `Router` component to use routing
- `Link`
  - Creates a (clickable) link that changes the URL of the browser when clicked
- `Route`
  - Defines components rendered based on the URL of the browser
  - Wrapped by the `Switch` component
- `Switch`
  - Renders the first component whose path matches the url of the browser
  - Note that the order of `Route` components is important
- `Redirect`

Example of `Redirect` component:

```js
<Route path="/settings">
  {hasSignedIn ? <Settings /> : <Redirect to="/login" />}
</Route>
```

### Parameterized Route

#### `useParams` Hook Function

Example:

```js
<Route path="/notes/:id">
  <Note />
</Route>
```

```js
import {
  // ...
  useParams,
} from "react-router-dom";

const Note = (props) => {
  const id = useParams().id;
  // ...
};
```

#### `useRouteMatch` Hook Function

`It is not possible to use useRouteMatch-hook in the component which defines the routed part of the application.`

Example:

```js
// index.js
ReactDOM.render(
  <Router>
    <App />
  </Router>,
  document.getElementById("root")
);
```

```js
// App.js
import { useRouteMatch } from "react-router-dom";

const App = () => {
  const match = useRouteMatch("/notes/:id");
  const note = match ? notes.find((note) => note.id === match.params.id) : null;
};
```

### `useHistory` Hook Function

With `useHistory` function, the component can access a history object, which can be used to modify the browser's url programmatically

Example:

```js
import {
  // ...
  useHistory,
} from "react-router-dom";

const Login = (props) => {
  const history = useHistory();

  const onSubmit = (event) => {
    event.preventDefault();
    // ...

    // Makes the url changed to `/`
    history.push("/");
  };

  return <form onSubmit={onSubmit}>{/* ... */}</form>;
};
```
