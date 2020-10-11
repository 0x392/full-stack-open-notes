# 5b. props.children and proptypes

[Back to index](../README.md)

## Reference to Components with `ref`

Example:

`App.js`

```js
import React, { useRef } from "react";
import Foo from "./Foo";

const App = () => {
  // 1. Call `useRef`
  //    Notice that `useRef` hook should be called
  //    inside of a function component
  const refFoo = useRef();

  const handleClick = () => {
    console.log("function `handleClick` in component `App` was invoked");

    console.log("`refFoo.current:`", refFoo.current);
    refFoo.current.bar();
  };

  // 2. Pass the reference to `Foo` component
  return (
    <div>
      <Foo ref={refFoo} />
      <button onClick={handleClick}>Button</button>
    </div>
  );
};
```

`userImperativeHandle`:

- It's a React hook
- Defines functions in a component which can be invoked from outside of the component

```js
// Foo.js
import React, { useImperativeHandle } from "react";

const bar = () => {
  console.log("function `bar` in component `Foo` was invoked");
};

// Notice using `React.forwardRef` to wrap the
// function component
const Foo = React.forwardRef((_props, ref) => {
  // 3. Use `useImperativeHandle` to expose
  //    inner functions
  useImperativeHandle(ref, () => ({
    first_prime: 2,
    seconde_prime: 3,
    bar,
  }));

  return <div>Component Foo</div>;
});

export default Foo;
```

## proptypes

Installation:

```shell
npm install prop-types --save
```

Example:

```js
import PropTypes from "prop-types";

LoginForm.propTypes = {
  handleSubmit: PropTypes.func.isRequired,
  handleUsernameChange: PropTypes.func.isRequired,
  handlePasswordChange: PropTypes.func.isRequired,
  username: PropTypes.string.isRequired,
  password: PropTypes.string.isRequired,
};
```
