# 6d. connect

[Back to index](../README.md)

## `connect` Function

- Provided by `react-redux`
- It's an older and more complicated way to use Redux

### Share the Redux Store to Components

- `connect` function can be used to transform "regular" React components into a "connected component"
- Replace `useSelector` with `mapStateToProps`
  - Mapping the state of Redux store into the component's props
- Replace `dispatch` with `mapDispatchToProps`
  - It's a group of action creators passed to the connected component as props

```js
// Using hook api of `react-redux`
import React from "react";
import { useDispatch, useSelector } from "react-redux";
import { toggleImportanceOf } from "../reducers/noteReducer";

// const Note = ...

const Notes = () => {
  const dispatch = useDispatch();
  const notes = useSelector(({ filter, notes }) => {
    if (filter === "ALL") {
      return notes;
    }
    return filter === "IMPORTANT"
      ? notes.filter((note) => note.important)
      : notes.filter((note) => !note.important);
  });

  return (
    <ul>
      {notes.map((note) => (
        <Note
          key={note.id}
          note={note}
          handleClick={() => dispatch(toggleImportanceOf(note.id))}
        />
      ))}
    </ul>
  );
};

export default Notes;
```

```js
// Using `connect` function of `react-redux`
import React from "react";
import { connect } from "react-redux";
import { toggleImportanceOf } from "../reducers/noteReducer";

// const Note = ...

const Notes = (props) => {
  const notesToShow = () => {
    if (props.filter === "ALL") {
      return props.notes;
    }
    return props.filter === "IMPORTANT"
      ? props.notes.filter((note) => note.important)
      : props.notes.filter((note) => !note.important);
  };

  return (
    <ul>
      {notesToShow().map((note) => (
        <Note
          key={note.id}
          note={note}
          handleClick={() => toggleImportanceOf(note.id)}
        />
      ))}
    </ul>
  );
};

// Defining the props of the connected component
// (based on the state of the Redux store)
const mapStateToProps = (state) => {
  return {
    notes: state.notes,
    filter: state.filter,
  };
};

const mapDispatchToProps = { toggleImportanceOf };

const ConnectedNotes = connect(mapStateToProps, mapDispatchToProps)(Notes);
export default ConnectedNotes;
```

### `mapDispatchToProps`

#### Object Form

The function passed in `mapDispatchToProps` must be action creators (functions that return Redux actions)

```js
const mapDispatchToProps = { toggleImportanceOf };
```

#### Function Form

```js
const mapDispatchToProps = (dispatch) => {
  return {
    createNote: (value) => {
      dispatch(createNote(value));
    },
  };
};
```

## Presentational and Container Components

- Presentational components
  - Are concerned with how things look
  - May contain both presentational and container components inside, and usually have some DOM markup and styles of their own
  - Often allow `props.children`
  - Have no dependencies on the rest of the app (e.g. Redux actions or stores)
  - Don't specify how the data is loaded or mutated
  - Receive data and callbacks via props exclusively
  - Rarely have their own state (when they do, it's UI state rather than data)
  - Are written as function components (unless they need state, lifecycle hooks or performance optimizations)
- Container components
  - Are concerned with how things work
  - May contain both presentational and container components inside, but usually don't have any DOM markup (except for some wrapping `<div>`s), and never have any styles
  - Provide the data and behavior to presentational or other container components
  - Call Redux actions and provide these as callbacks to the presentational components
  - Are often stateful,as they tend to serve as data source
  - Are usually generated using **higher order components** such as `connect` from `react-redux`, rather than written by hand

### High Order Component (HOC)

A high order component is a function that

- Accepts a "regular" component as its parameter
- Returns a new "regular" component

## Redux-like State Management

- [React Context api](https://reactjs.org/docs/context.html)
- [React useReducer hook](https://reactjs.org/docs/hooks-reference.html#usereducer)
