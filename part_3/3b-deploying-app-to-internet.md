# 3b. Deploying app to internet

[Back to index](../README.md)

## CORS

CORS: Cross-Origin Resource Sharing

- Restricted resources
  - e.g. fonts, AJAX requests
  - Forbidden by default by the same-origin security policy
- Non-restricted resources
  - e.g. images, stylesheets, scripts, iframes, videos

Note: `localhost:3000` and `localhost:3001` are in different origins

## When to Use `cors`, `static`, Proxy

### `cors` Middleware

Installation:

```shell
npm install cors --save
```

- Scenario
  - For development
  - Frontend development server: `localhost:3000`
    - The page is not served as by the same server of the backend
    - Without using the proxy setting of `create-react-app`
    - API path declared in `services/notes.js`: using absolute path as following
  - Address to backend API:
    - `localhost:3001/api/notes` or
    - `<heroku-server>/api/notes`
- Problem: API request is blocked by CORS policy
- Solution: add `app.use(cors());` to backend server

### `static` Middleware

- For production
- To serve static file from the backend, add the following code to backend server

```js
app.use(express.static("build"));
```

### Proxy

- Scenario:
  - For development
  - Frontend development server: `localhost:3000`
    - Created by `create-react-app`
    - API path declared in `services/notes.js`: `/api/notes` (relative path)
  - Address to backend API: `localhost:3001/api/notes`
- Problem: "404 Not Found" due to wrong address (`localhost:3000/api/notes`) requested by the browser
- Solution: Add `"proxy": "http://localhost:3001"` to `package.json` of frontend
- Note that we still need to handle the issue of CORS policy
