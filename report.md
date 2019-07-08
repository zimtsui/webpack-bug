<!-- Please don't delete this template because we'll close your issue -->
<!-- Before creating an issue please make sure you are using the latest version of webpack. -->

# Bug report

<!-- Please ask questions on StackOverflow or the webpack Gitter. -->
<!-- https://stackoverflow.com/questions/ask?tags=webpack -->
<!-- https://gitter.im/webpack/webpack -->
<!-- Issues which contain questions or support requests will be closed. -->

**What is the current behavior?**
```shell
TypeError: __webpack_require__(...) is not a function
```

**If the current behavior is a bug, please provide the steps to reproduce.**

it's a really simple situation that takes you 5 minutes to check the entire codes.

```js
// ./src/index.js
require('./other');


// ./src/other.js
module.exports = { a: 1 };


// ./babel.config.js
module.exports = {
    "presets": ["@babel/preset-env"],
    "plugins": ["babel-plugin-source-map-support"]
};


// ./webpack.config.js
const path = require('path');
const { CleanWebpackPlugin } = require('clean-webpack-plugin');
const webpackNodeExternals = require('webpack-node-externals');

module.exports = {
    mode: 'development',
    target: 'node',
    devtool: 'source-map',
    entry: './src/index.js',
    output: {
        filename: 'index.js',
        path: path.resolve(__dirname, 'dist'),
    },
    module: {
        rules: [{ test: /\.js$/, use: 'babel-loader' }],
    },
    plugins: [
        new CleanWebpackPlugin(),
    ],
    externals: [webpackNodeExternals()],
};
```

```json
// package.json
{
  "name": "webpack-bug",
  "version": "0.0.1",
  "description": "",
  "main": "./dist/index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack"
  },
  "author": "",
  "license": "UNLICENSED",
  "devDependencies": {
    "@babel/cli": "^7.5.0",
    "@babel/core": "^7.5.0",
    "@babel/preset-env": "^7.5.0",
    "babel-loader": "^8.0.6",
    "babel-plugin-source-map-support": "^2.0.1",
    "clean-webpack-plugin": "^3.0.0",
    "webpack": "^4.35.2",
    "webpack-cli": "^3.3.5",
    "webpack-node-externals": "^1.7.2"
  },
  "dependencies": {
    "source-map-support": "^0.5.12"
  }
}
```

you can clone the repo for checking.

```shell
$git clone git@github.com:zimtsui/webpack-bug.git
$cd ./webpack-bug
$npm i
$npm run build
$node .
TypeError: __webpack_require__(...) is not a function
```

`babel-plugin-source-map-support` is a simple babel plugin, which just add a single line at the beginning.

```shell
$npx babel ./src/index.js
"use strict";

require("source-map-support/register"); // the plugin adds this line

require('./other');
```

if you disable the plugin and manually add that line in source file, everything will be ok:

```js
// ./babel.config.js
module.exports = {
    "presets": ["@babel/preset-env"],
    // "plugins": ["babel-plugin-source-map-support"]
};


// ./src/index.js
require('source-map-support/register');
require('./other');
```


<!-- A great way to do this is to provide your configuration via a GitHub repository -->
<!-- The most helpful is a minimal reproduction with instructions on how to reproduce -->
<!-- Repositories with too many files or large `webpack.config.js` files are not suitable -->
<!-- Please only add small code snippets directly into this issue -->
<!-- https://gist.github.com is a good place for longer code snippets -->
<!-- If your issue is caused by a plugin or loader, please create an issue on the loader/plugin repository instead -->

**What is the expected behavior?**
to exit normally and output nothing.

<!-- "It should work" is not a helpful explanation -->
<!-- Explain exactly how it should behave -->

**Other relevant information:**
webpack version:  4.35.2
Node.js version: 10.16.0 lts
Operating System: ubuntu 18.04
Additional tools:

