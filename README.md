# Learn_Express
A tutorial to learn express.

## What is node.js?
Javascript is a popular scripting language. A web browser is needed to run a javascript code. To run javascript outside of the web browser, Node has been developed. It has been built on top of an opensource chrome v8 javascript engine. 

## Managing packages with NPM
npm is a package manager for the JavaScript programming language. It is the default package manager for the JavaScript runtime environment Node.js. 

When we do `npm init`, we initiaze a node project. It adds an package.json file inside the directory of our project. 

This package.json communicates with npm about all the extranal depenancies of our project. It also tells about the author, description, keywords, license, version, dependencies (~(patch updates automatically), ^(minor updates automatically)).

### Semantic versioning
"package": "MAJOR.MINOR.PATCH"

The MAJOR version should increment when you make incompatible API changes. The MINOR version should increment when you add functionality in a backwards-compatible manner. The PATCH version should increment when you make backwards-compatible bug fixes. This means that PATCHes are bug fixes and MINORs add new features but neither of them break what worked before. Finally, MAJORs add changes that won’t work with earlier versions.

## Express
Node.js is a javascript runtime which allows us to write backend programs in javascript. It has some built-in modules to aid in writing server side programs. Express, while not included in Node.js, is another module used with it. Express runs between the server created by Node.js and the frontend pages of a web application. Express also handles an application's routing. Routing directs users to the correct page based on their interaction with the application. 

We can start a node server by typing `node file_name.js`.

### Some basic commands -
1. First we need to create an express app object.
   ```
   var express = require('express');
   var app = express();
   ```
   require('xxx.js'), where the .js extension can be omitted, returns whatever is exported by that xxx.js file. If that xxx.js file exports an object, require('xxx.js') returns an object; if a function is exported, require('xxx.js') returns a function; if a single string is exported, require('xxx.js') returns a string...
   If you check source code of file express.js, you will see that it exports a single function. So in `var express = require('express')`.
   The first express is assigned whatever is exported by module express, which in this case happens to be a single function.
   express is a function, not a reference to a module. Hence on second row you just invoke that function: `var app = express()`.
2. 

## Sources -
1. FreeCodeCamp
