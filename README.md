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
