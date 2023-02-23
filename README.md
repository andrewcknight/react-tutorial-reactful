https://jscomplete.com/learn/1rd-reactful

1. `npm init --y`
2. `npm i express`
3. `npm i react react-dom`
4. `npm i webpack webpack-cli`
5. `npm i babel-loader @babel/core @babel/node @babel/preset-env @babel/preset-react`
6. `npm i -D nodemon`
7. `npm i -D eslint @babel/eslint-parser eslint-plugin-react eslint-plugin-react-hooks`

Create file ".eslintrc.js"
```
module.exports = {
  parser: "@babel/eslint-parser",
  env: {
    browser: true,
    commonjs: true,
    es6: true,
    node: true,
    jest: true,
  },
  parserOptions: {
    ecmaVersion: 2020,
    ecmaFeatures: {
      impliedStrict: true,
      jsx: true,
    },
    sourceType: "module",
  },
  plugins: ["react", "react-hooks"],
  extends: [
    "eslint:recommended",
    "plugin:react/recommended",
    "plugin:react-hooks/recommended",
  ],
  settings: {
    react: {
      version: "detect",
    },
  },
  rules: {
    "react/prop-types": "off",
    "react/react-in-jsx-scope": "off",

    // You can do more rule customizations here...
  },
};
```

Create directory structure
```
reactful/
  dist/
    main.js
  src/
    index.js
    components/
      app.js
    server/
      server.js
```

Create "babel.config.js"
```
module.exports = {
  presets: [
    "@babel/preset-env",
    ["@babel/preset-react", { "runtime": "automatic" }]
  ],
};
```

Create "webpack.config.js"
```
module.exports = {
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader",
        },
      },
    ],
  },
};
```

Add scripts to package.json
```
 "dev:server": "nodemon --exec babel-node src/server/server.js --ignore dist/",
 "dev:bundler": "webpack -w --mode=development"
```

Create "src/components/App.js" file
```
import { useState } from "react";

export default function App() {
  const [count, setCount] = useState(0);
  return (
    <div>
      This is a sample stateful and server-side
      rendered React application.
      <br />
      <br />
      Here is a button that will track
      how many times you click it:
      <br />
      <br />
      <button onClick={() => setCount(count + 1)}>{count}</button>
    </div>
  );
}
```

Create "src/index.js" file
```
import ReactDOM from "react-dom/client";

import App from "./components/app";

const container = document.getElementById("app");
ReactDOM.hydrateRoot(container, <App />);
```

Create "src/server/server.js" file
```
import express from "express";
import ReactDOMServer from "react-dom/server";

import App from "../components/app";

const server = express();
server.use(express.static("dist"));

server.get("/", (req, res) => {
  const initialMarkup = ReactDOMServer.renderToString(<App />);

  res.send(`
    <html>
      <head>
        <title>Sample React App</title>
      </head>
      <body>
        <div id="app">${initialMarkup}</div>
        <script src="/main.js"></script>
      </body>
    </html>
  `)
});

server.listen(4242, () => console.log("Server is running..."));
```

Run in two separate terminals:
```
npm run dev:server
npm run dev:bundler
```