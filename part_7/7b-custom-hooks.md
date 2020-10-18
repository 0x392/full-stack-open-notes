# 7b. Custom hooks

[Back to index](../README.md)

## Examples of Hooks

### React Built-in Hooks

React offers 10 built-in hooks

- `useState`
- `useEffect`
- `useImperativeHandle`
  - Allows components to provide their functions to other components

### `react-redux`

- `useSelector`
- `useDispatch`

### React Router

- `useParams`
- `useRouteMatch`
- `useHistory`

## Hooks Rules

- Don't call hooks from regular JavaScript function
  - Call hooks from React function components
  - Call hooks from custom hooks
- There's an ESLint rule `eslint-plugin-react-hooks` that can be used to verify that the application uses hooks correctly

## Custom Hooks

- Purpose: extract component logic into reusable functions
- Custom hooks are regular JavaScript functions
  - They can use any other hooks
  - The name of custom hooks must start with `use`

### Example: Counter

```js
// index.js
import { useCounter } from "./hooks";

const Counter = (props) => {
  const counter = useCounter();

  return (
    <div>
      <div>{props.name}</div>
      <div>{counter.value}</div>
      <button onClick={counter.increase}>Increase</button>
      <button onClick={counter.decrease}>Decrease</button>
      <button onClick={counter.zero}>Zero</button>
    </div>
  );
};

const App = () => {
  return (
    <>
      <Counter name="foo" />
      <Counter name="bar" />
    </>
  );
};
```

```js
// hooks/index.js
import { useState } from "react";

export const useCounter = () => {
  const [value, setValue] = useState(0);

  const increase = () => setValue(value + 1);
  const decrease = () => setValue(value - 1);
  const zero = () => setValue(0);

  return { value, increase, decrease, zero };
};
```

### Example: Input Field and Spread Attributes

```js
// index.js
import { useField } from "./hooks";

const App = () => {
  const field = useField("text");

  return (
    <>
      <div>
        <input
          type={field.type}
          value={field.value}
          onChange={field.onChange}
        />
      </div>
      <div>{field.value}</div>
    </>
  );
};
```

```js
// hooks/index.js
import { useState } from "react";

export const useFiled = (type) => {
  const [value, setValue] = useState("");

  const onChange = (event) => {
    setValue(event.target.value);
  };

  return { type, value, onChange };
};
```

## More about Hooks

- [Awesome React Hooks](https://github.com/rehooks/awesome-react-hooks)
- [Easy to understand React Hook recipes](https://usehooks.com/)
- [Why Do React Hooks Rely on Call Order?](https://overreacted.io/why-do-hooks-rely-on-call-order/)
