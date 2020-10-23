# 7e. Class components, miscellaneous

[Back to index](../README.md)

## Class Components

The correct place to put the code that fetches data from a server:

- Function components : effect hook
- Class components: lifecycle methods
  - `componentDidMount`: executed once right after the first time component render

Example:

```js
import React from "react";
import axios from "axios";

class App extends React.Component {
  constructor(props) {
    super(props);

    // Class component contains only one state
    this.state = {
      notes: [],
      currentIdx: 0,
    };
  }

  componentDidMount = () => {
    axios.get("http://localhost:3001/api/notes").then((response) => {
      this.setState({ notes: response.data });
    });
  };

  handleClick = () => {
    const currentIdx = Math.floor(Math.random() * this.state.notes.length);
    this.setState({ currentIdx });
  };

  render() {
    return (
      <div>
        <div>{this.state.notes[this.state.currentIdx]}</div>
        <button onClick={this.handleClick}>Next</button>
      </div>
    );
  }
}

export default App;
```

## Organization Code of React App

There is no correct way to organize a project

One common way to organize the code:

```plaintext
├── components
├── reducers
├── services
```

## Notifying the Frontend of the Backend Changes

- Polling
- WebSockets
  - [Socket.io](https://socket.io/)
- Mechanism provided by GraphQL

## Security Issues

- Injection
  - Prevented by sanitizing inputs
- Cross-site scripting (XSS)
  - Security updates to libraries
    - `npm audit`

## Current Trends

### Server-side Rendering

- React server-side rendering: when accessing the application for the first time, the server serves a pre-rendered page made with React
- For SEO(Search Engine Optimization)

### Isomorphic Applications and Universal Code

[Next.js](https://github.com/vercel/next.js)

### Progressive Web App (PWA)

Service workers 🤔

### Microservice Architecture

Ways to compose the backend:

- Monolithic backend
- Microservice architecture

Microservice architecture:

- Independent services communicate each other with the network
- An individual service takes care of a particular logic functional whole

### Serverless
