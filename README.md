# Starter Base Node/Express SPA Project

To get started with this project, you will need to make sure that you update your installation of Node so that it's at least version 15.0.0 LTS or later.  I'm trying to keep this project as OS agnostic as I can and if there are any differences between Windows, MacOS, or Linux and I will try to make note of what is required for eash language.

This is the tech stack that we're going to be working with for this project:

* NodeJS
* ExpressJS
* Webpack
* Babel
* CSS

Te get started, let's create a project directory that is going to house all of code that we need to muild the final project:

```shell
mkdir myproject
cd myproject
```

## 1.) Install Express

```shell
npm init -y
npm install express
```

## 2.) Create Express Server File

Create a file in your project directory called server.js and add this following code:

```javascript
const path = require('path');
const express = require('express');

const app = express(), index = path.join(__dirname, 'index.html');
app.use(express.static(__dirname))
app.get('*', (req, res) => {
  res.sendFile(index);
});

const PORT = process.env.PORT || 8000;

app.listen(PORT, () => {
  console.log(`App listening to ${PORT}...`);
  console.log('Press Ctrl+C to quit.');
});
```

## 3.) Create HTML file

Create a file in your project directory called: index.html and add the following code:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <title>Base Node Express App</title>
</head>
<body>
    <h1>Hello World!</h1>
</body>
</html>
```

## 4.) Test your work

```shell
npm start
```

If there are no errors reported when you run "npm start", then open your browser to:  <http://localhost:8000> and you should see your "Hello World!" page.

## 5.) Install Webpack

We're now going to install Webpack and a couple of extra modules, the CLI module is needed to run Webpack from the command line.  The module "webpack-node-externals" is required to define modules that you don't want bundled with your application.  This is specific to the "node-modules" directory in node applications.  The "html-webpack-plugin" is used to help make the creation of HTML much simpler.  The module "html-webpack-plugin" is needed to copy over our required files to the "dist" directory.

```shell
npm install --save-dev webpack webpack-cli webpack-node-externals html-webpack-plugin
```

## 6.) Install Babel

Now, we'll install Babel which is used to transpile ES6^ features to ES5 and the "html-loader" module is used to insert a script tag in your HTML file that will import the transpiled main Javascript file.

```shell
npm install --save-dev @babel/core @babel/preset-env babel-loader html-loader
```

## 7.) Create Webpack config file

Create a file in your project directory called: webpack.config.js and add the following code:

```javascript
const path = require('path')
const webpack = require('webpack')
const nodeExternals = require('webpack-node-externals')
const HtmlWebPackPlugin = require("html-webpack-plugin")
module.exports = {
  entry: {
    server: './server.js',
  },
  output: {
    path: path.join(__dirname, 'dist'),
    publicPath: '/',
    filename: '[name].js'
  },
  target: 'node',
  node: {
     __dirname: false,
    __filename: false,
  },
  externals: [nodeExternals()],
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader"
        }
      },
      {
        test: /\.html$/,
        use: [{loader: "html-loader"}]
      }
    ]
  },
  plugins: [
    new HtmlWebPackPlugin({
      template: "./index.html",
      filename: "./index.html",
      excludeChunks: [ 'server' ]
    })
  ]
}
```

## 8.) Create a Babel config file

Create a file in your project directory called: .babelrc and add the following code:

```javascript
{
  "presets": ["@babel/preset-env"]
}
```

## 9.) Fix the server.js file

Create a file in your project directory called: .babelrc and add the following code:

```javascript
const path = require('path');
const express = require('express');
```

to

```javascript
import path from 'path';
import express from 'express';
```

## 10.) Modify package.json scripts parameters

We now have to modify the packagae.json file:

```javascript
 "scripts": {
    "start": "node ./server.js"
  },
```

to

```javascript
 "scripts": {
    "build": "rmdir dist && webpack --mode development",
    "start": "node ./dist/server.js"
  },
```

## 11.) Add CSS and Javascript to app build

```shell
npm install --save-dev css-loader file-loader style-loader
```

## 12.) Modify package.json scripts parameters

We now have to modify the packagae.json file once again:

```javascript
 "scripts": {
    "build": "rmdir dist && webpack --mode development",
    "start": "node ./dist/server.js"
  },
```

to

```javascript
 "scripts": {
    "build": "rmdir dist && webpack --mode development --config webpack.server.config.js && webpack --mode development",
    "start": "node ./dist/server.js"
  },
```

## 13.) Update HTML file

We now have to modify the index.html file:

```html
<body>
    <h1>Hello World!</h1>
</body>
```

to

```html
<body>
    <h1>Hello World!</h1>
    <div class="test-class"></div>
</body>
```

## 14.) Create CSS file

Create ./src/css/style.css file and add the following code:

```css
h1, h2, h3, h4, h5, p {
  font-family: helvetica;
  color: #3e3e3e;
}
.desc {
  font-size: 14px;
  color: #9e9e9e;
}
.test-class {
  width: 300px;
  height: 300px;
  background-size: 100% auto;
  background-repeat: no-repeat;
}
```

## 15.) Create index.js file

Create ./src/index.js file and add the following code:

```javascript
import logMessage from './js/logger';
import './css/style.css';
logMessage('Testing logging feature!');
```

## 16.) Create logger.js file

Create ./src/js/logger.js file and add the following code:

```javascript
const logMessage = msg => console.log(msg);
export default logMessage;
```
