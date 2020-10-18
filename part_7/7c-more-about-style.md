# 7c. More about styles

[Back to index](../README.md)

## `react-bootstrap`

Installation:

```shell
npm install react-bootstrap --save
```

- In Bootstrap, contents are typically rendered inside of a "container"
- Give the root `div` element the `container` class attribute

### `react-bootstrap` Examples

Table:

```js
import { Table } from "react-bootstrap";

<Table striped>
  <tbody>{/* ... */}</tbody>
</Table>;
```

Form and button:

```js
import { Form, Button } from "react-bootstrap";

<Form>
  <Form.Group>
    <Form.Label>Username</Form.Label>
    <Form.Control type="text" name="username" />
    <Form.Label>Password</Form.Label>
    <Form.Control type="password" name="password" />
    <Button variant="primary" type="submit">
      Login
    </Button>
  </Form.Group>
</Form>;
```

Alert (notification):

```js
import { Alert } from "react-bootstrap";

const [message, setMessage] = useState(null);

{
  message && <Alert variant="success">{message}</Alert>;
}
```

## Material-UI

Installation:

```shell
npm install @material-ui/core --save
```

Load Google's font Roboto:

```html
<link
  rel="stylesheet"
  href="https://fonts.googleapis.com/css?family=Roboto:300,400,500,700&display=swap"
/>
```

Contents are wrapped within a `Container` component:

```js
import Container from "@material-ui/core/Container";

const App = () => {
  return <Container>{/* ... */}</Container>;
};
```

### Material-UI Examples

Table:

```js
import {
  TableContainer,
  Paper,
  Table,
  TableBody,
  TableRow,
  TableCell,
} from "@material-ui/core";

<TableContainer component={Paper}>
  <Table>
    <TableBody>
      {notes.map((note) => (
        <TableRow key={note.id}>
          <TableCell>
            <Link to={`/notes/${note.id}`}>{note.content}</Link>
          </TableCell>
          <TableCell>{note.user}</TableCell>
        </TableRow>
      ))}
    </TableBody>
  </Table>
</TableContainer>;
```

Form (`TextField` and `Button` components):

```js
import { TextField, Button } from "@material-ui/core";

<form>
  <div>
    <TextField label="username" />
  </div>
  <div>
    <TextField label="password" type="password" />
  </div>
  <Button variant="contained" color="primary" type="submit">
    Login
  </Button>
</form>;
```

Alert (notification):

Note that we have to install the package by `npm install @material-ui/lab --save`

```js
import { Alert } from "@material-ui/lab";

const [message, setMessage] = useState(null);

{
  message && <Alert severity="success">{message}</Alert>;
}
```

Component prop:

```js
<Button color="inherit" component={Link} to="/">
  home
</Button>
```

- The `Button` component is rendered as a child component of the `Link` component
- The `Link` component receives the `to` prop

## Styled Components

Installation:

```shell
npm install styled-components --save
```

Example:

```js
import styled from "styled-components";

const Button = styled.button`
  border: 1px solid #000;
  padding: 1rem;
`;

<Button type="submit">Login</Button>;

const Input = styled.input`
  padding: 0.25rem;
`;

<Input type="password" />;

const Nav = styled.div`
  padding: 1rem;
`;

<Nav>{/* Links */}</Nav>;
```
