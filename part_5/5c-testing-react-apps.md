# 5c. Testing React apps

[Back to index](../README.md)

## Testing Libraries

Jest: configured by default to apps created with `create-react-app`

### `react-testing-library`

Installation:

```shell
npm install @testing-library/react --save-dev

# `@testing-library/jest-dom` is for the custom jest
# matchers
npm install @testing-library/jest-dom --save-dev
```

### Example: Testing File for `Note` Component

Component: `src/components/Note.js`

```js
const Note = ({ note, toggleImportance }) => {
  const label = note.important ? "make not important" : "make important";

  return (
    <li className="note">
      {note.content}
      <button onClick={toggleImportance}>{label}</button>
    </li>
  );
};
```

Testing file: `src/components/Note.test.js`

```js
import React from "react";
import { render } from "@testing-library/react";
import "@testing-library/jest-dom/extend-expect";
import Note from "./Note";

test("renders content", () => {
  const note = {
    content: "Component testing is done with react-testing-library",
    important: true,
  };

  const component = render(<Note note={note} />);

  // Method 1
  expect(component.container).toHaveTextContent(
    "Component testing is done with react-testing-library"
  );

  // Method 2
  const element = component.getByText(
    "Component testing is done with react-testing-library"
  );
  expect(element).toBeDefined();

  // Method 3
  const li = component.container.querySelector(".note");
  expect(li).toHaveTextContent(
    "Component testing is done with react-testing-library"
  );
});
```

The object returned by `render` has several properties and methods:

- `container`: contains all of the HTML rendered by the component
  - `.querySelector(..)`
- `.getByText(text)`: return the element that contains the given text
- [More query methods](https://testing-library.com/docs/dom-testing-library/api-queries)

Matcher methods provided by `jest-dom` library:

- `.toHaveStyle(..)`
- `.toHaveTextContent(..)`

### Running Tests

- Watch mode by default (`create-react-app`)
- Run tests not in watch mode: `CI=true npm test`

### Debugging Tests

```js
import { prettyDOM } from "@testing-library/dom";
/* Import other libraries... */

test("renders content", () => {
  /* ... */

  const component = render(<Note note={note} />);

  // Prints the HTML rendered by the component
  component.debug();

  const li = component.container.querySelector("li");
  console.log(prettyDOM(li));
});
```

Using `prettyDOM` (imported from `@testing-library/dom`) to print HTML of the smaller part of a component

### Clicking Button in Tests

```js
import { render, fireEvent } from "@testing-library/react";
/* Import other libraries... */

test("clicking the button calls event handler once", () => {
  const note = {
    content: "Component testing is done with react-testing-library",
    important: true,
  };

  const mockHandler = jest.fn();

  const component = render(<Note note={note} toggleImportance={mockHandler} />);

  const button = component.getByText("make not important");
  fireEvent.click(button);

  expect(mockHandler.mock.calls).toHaveLength(1);
});
```

- Mock objects and functions
  - Used to replace dependencies of the components being tests
  - Can return hardcoded response
  - Can verify the number of times the mock functions are called

### Testing the Form

```js
import { render, fireEvent } from "@testing-library/react";
/* Import other libraries... */

test("<NoteForm /> updates parent state and calls onSubmit", () => {
  const createNote = jest.fn();

  const component = render(<NoteForm createNote={createNote} />);

  const input = component.container.querySelector("input");
  const form = component.container.querySelector("form");

  // Simulate writing to the input field
  fireEvent.change(input, {
    target: { value: "testing of forms could be easier" },
  });
  fireEvent.submit(form);

  expect(createNote.mock.calls).toHaveLength(1);

  // `createNote.mock.calls[m][n]`:
  //   the n-th parameter of the m-th function call
  expect(createNote.mock.calls[0][0].content).toBe(
    "testing of forms could be easier"
  );
});
```

## Snapshot Testing
