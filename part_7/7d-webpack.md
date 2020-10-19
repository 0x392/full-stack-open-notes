# 7d. Webpack

[Back to index](../README.md)

## Webpack

Installation:

```shell
npm install webpack webpack-cli --save-dev
```

Loaders:

- `@babel/core babel-loader`
- `@babel/preset-env`
  - Transform the latest features of JavaScript into code compatible with ES5 standard
- `@babel/preset-react`
  - Transform JSX code into regular JavaScript
- `style-loader`
  - Generate and inject a `<style>` element that contains all of the styles
- `css-loader`
  - Load the CSS files

### File Structure

```plaintext
├── build
├── package.json
├── src
│   ├── App.js
│   └── index.js
└── webpack.config.js
```

#### `package.json`

```json
{
  "scripts": {
    "build": "webpack --mode=development"
  }
}
```

#### `webpack.config.js`

```js
const path = require("path");

const config = {
  entry: "./src/index.js",
  output: {
    path: path.resolve(__dirname, "build"),
    filename: "main.js",
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        loader: "babel-loader",
        options: {
          presets: ["@babel/preset-env", "@babel/preset-react"],
        },
      },
      {
        test: /\.css$/,
        // The order of `style-loader` and
        // `css-loader` matters
        use: ["style-loader", "css-loader"],
      },
    ],
  },
  stats: { preset: "normal", colors: true },
};

module.exports = config;
```

### `webpack-dev-server`

Installation:

```shell
npm install webpack-dev-server --save-dev
```

Add the script to `package.json`:

```json
{
  "scripts": {
    "start": "webpack serve --mode=development"
  }
}
```

Then, add `devServer` property to `webpack.config.js`:

```js
const config = {
  // ...
  devServer: {
    contentBase: path.resolve(__dirname, "build"),
    compress: true,
    port: 3000,
  },
};
```

The result of the bundling exists only in memory

### Source Maps

Add `devtool` property to `webpack.config.js`:

```js
const config = {
  // ...
  devtool: "source-map",
};
```

### Minifying the Code

Starting from version 4 of webpack, the minification plugin does not require additional configuration

Modify `package.json` to make webpack execute in production mode:

```json
{
  "scripts": {
    "build": "webpack --mode=production"
  }
}
```

### Webpack DefinePlugin

Example:

`webpack.config.js`:

```js
const webpack = require("webpack");

const config = (env, argv) => {
  const backend_url =
    argv.mode === "production"
      ? "https://myapp.io/api"
      : "http://localhost:3001/api";

  return {
    // ...
    plugins: [
      new webpack.DefinePlugin({
        BACKEND_URL: JSON.stringify(backend_url),
      }),
    ],
  };
};
```

`App.js`:

```js
const App = () => {
  const service = useService(BACKEND_URL);

  return <div>Server: {BACKEND_URL}</div>;
};
```

Note: [separate configuration files](https://webpack.js.org/guides/production/)

## Transpilation and Polyfill

- Transpilation: transforms the code from a newer version of JavaScript to an older one
- Polyfill: Add the missing functionality to old browsers
- Resources
  - [babel-polyfill](https://babeljs.io/docs/en/babel-polyfill/)
  - An exhaustive polyfill list: [here](https://github.com/Modernizr/Modernizr/wiki/HTML5-Cross-browser-Polyfills)
  - Check the browser compatibility: [https://caniuse.com/](https://caniuse.com/)

## Other Resources

- Inspect the app locally: `npx static-server`
- [mini-css-extract-plugin](https://github.com/webpack-contrib/mini-css-extract-plugin)
