# Learn_Express
A tutorial to learn express.

Add things from -
1) Challenges - https://stackoverflow.com/questions/23114374/file-uploading-with-express-4-0-req-files-undefined

## What is node.js?
Javascript is a popular scripting language. A web browser is needed to run a javascript code. To run javascript outside of the web browser, Node has been developed. It has been built on top of an opensource chrome v8 javascript engine. 

## Managing packages with NPM
npm is a package manager for the JavaScript programming language. It is the default package manager for the JavaScript runtime environment Node.js. 

When we do `npm init`, we initiaze a node project. It adds a package.json file inside the directory of our project. 

This package.json communicates with npm about all the external dependencies of our project. It also tells about the author, description, keywords, license, version, dependencies (~(patch updates automatically), ^(minor updates automatically)).

### Semantic versioning
"package": "MAJOR.MINOR.PATCH"

The MAJOR version should increment when we make incompatible API changes. The MINOR version should increment when we add functionality in a backwards-compatible manner. The PATCH version should increment when we make backwards-compatible bug fixes. This means that PATCHes are bug fixes and MINORs add new features but neither of them break what worked before. Finally, MAJORs add changes that won’t work with earlier versions.

## Express
Node.js is a javascript runtime which allows us to write backend programs in javascript. It has some built-in modules to aid in writing server side programs. Express, while not included in Node.js, is another module used with it. Express runs between the server created by Node.js and the frontend pages of a web application. Express also handles an application's routing. Routing directs users to the correct page based on their interaction with the application. 

We can start a node server by typing `node file_name.js`.

### Some basic commands -
First we need to create an Express app object.
```
var express = require('express');
var app = express();
```
require('xxx.js'), where the .js extension can be omitted, returns whatever is exported by that xxx.js file. If that xxx.js file exports an object, require('xxx.js') returns an object; if a function is exported, require('xxx.js') returns a function; if a single string is exported, require('xxx.js') returns a string...

If we check the source code of file express.js, we will see that it exports a single function. So in `var express = require('express')`.

The first express is assigned whatever is exported by module express, which in this case happens to be a single function.
express is a function, not a reference to a module. Hence on second row we just invoke that function: `var app = express()`.

This Express app object has many functions. We'll cove them in the following points -

1. **app.listen(port)** - This tells our server to listen to a specific port. By default node server listens on port 3000.

2. **app.get(path, handler)** - In express, a route takes the following structure. In place of get we can have any http method, like get, put, post, delete. Path is a relative on the server, it can be a string or even a regular expression. Handler is a function that Express calls when the route is matched. Handlers take the form function(req, res) {...}, where req is the request object, and res is the response object. For example, the handler
   ```
   function(req, res) {
     res.send('Response String');
   }
   ```
   will serve the string 'Response String'.
   
3. **res.sendFile(path)** - We can respond to a request with a file using the sendFile method. We can put this in any express route handler method. Behind the scenes, this method will set the appropriate headers to instruct your browser on how to handle the file you want to send, according to its type. Then it will read and send the file. This method needs an absolute file path. We can use the Node global variable \_\_dirname to calculate the path.`absolutePath = __dirname + relativePath/file.ext`.

4. **app.use(path, middlewareFunction)** - Middleware are functions that intercept route handlers, adding some kind of information. A middleware needs to be mounted using this method. The first path argument is optional. If we don’t pass it, the middleware will be executed for all requests. Some of the middlewareFunctions are - 

   a. **express.static(path)** - An HTML server usually has one or more directories that are accessible by the user. We can place there the static assets needed by your application (stylesheets, scripts, images). In Express, you we put in place this functionality using this middleware function, where the path parameter is the absolute path of the folder containing the assets.
   b. Middleware functions are functions that take 3 arguments: the request object, the response object, and the next function in the application’s request-response cycle. These functions execute some code that can have side effects on the app, and usually add information to the request or response objects. They can also end the cycle by sending a response when some condition is met. If they don’t send the response when they are done, they start the execution of the next function in the stack. This triggers calling the 3rd argument, next(). 
      ```
      function(req, res, next) {
        console.log("I'm a middleware...");
        next();
      }
      ```
      Let’s suppose we mounted this function on a route. When a request matches the route, it displays the string “I’m a middleware…”, then it executes the next function in the stack. We can use the `app.use(<mware-function>)` method. In this case, the function will be executed for all the requests, but we can also set more specific conditions. For example, if we want a function to be executed only for POST requests, we could use `app.post(<mware-function>)`. Analogous methods exist for all the HTTP verbs (GET, DELETE, PUT, …). Remember to call next() when done, or our server will be stuck forever.
      Note - Express evaluates functions in the order they appear in the code. This is true for middleware too. If you want it to work for all the routes, it should be mounted before them.
   c. Middleware can also be chained inside route definition.
      ```
      app.get('/user', function(req, res, next) {
        req.user = getTheUserSync();  // Hypothetical synchronous operation
        next();
      }, function(req, res) {
        res.send(req.user);
      });
      ```
      This approach is useful to split the server operations into smaller units. That leads to a better app structure, and the possibility to reuse code in different places. This approach can also be used to perform some validation on the data. At each point of the middleware stack you can block the execution of the current chain and pass control to functions specifically designed to handle errors. Or you can pass control to the next matching route, to handle special cases.

5. **res.json({key: data})** - We can use Express to create REST APIs and send json using this command. We can pass any object as an argument. This method closes the request-response loop, returning the data. Behind the scenes, it converts a valid JavaScript object into a string, then sets the appropriate headers to tell our browser that we are serving JSON, and sends the data back. A valid object has the usual structure {key: data}. Data can be a number, a string, a nested object or an array. Data can also be a variable or the result of a function call, in which case it will be evaluated before being converted into a string.

6. **req** - We can get the request method (http verb), the relative route path, and the caller’s ip from the request object using `req.method`, `req.path` and `req.ip`. 
   Route parameters are named segments of the URL, delimited by slashes (/). Each segment captures the value of the part of the URL which matches its position. The captured values can be found in the `req.params` object. 
   ```
   route_path: '/user/:userId/book/:bookId'
   actual_request_URL: '/user/546/book/6754'
   req.params: {userId: '546', bookId: '6754'}
   ```
   Query string is delimited by a question mark (?), and includes field=value couples. Each couple is separated by an ampersand (&). Express can parse the data from the query string, and populate the object `req.query`. Some characters, like the percent (%), cannot be in URLs and have to be encoded in a different format before you can send them. If you use the API from JavaScript, you can use specific methods to encode/decode these characters.
   ```
   route_path: '/library'
   actual_request_URL: '/library?userId=546&bookId=6754'
   req.query: {userId: '546', bookId: '6754'}
   ```
   For POST requests, the data doesn’t appear in the URL, it is hidden in the request body. This is a part of the HTML request, also called payload. Since HTML is text-based, even if we don’t see the data, it doesn’t mean that it is secret. For this we need to install the `body-parser` package. `var bodyParser = require('body-parser');`. This package allows us to use a series of middleware, which can decode data in different formats.The middleware to handle urlencoded data is returned by `bodyParser.urlencoded({extended: false})`.Extended=false is a configuration option that tells the parser to use the classic encoding. When using it, values can be only strings or arrays. The extended version allows more data flexibility, but it is outmatched by JSON. After this, we will find the parameters in the object `req.body`.
   ```
   route: POST '/library'
   urlencoded_body: userId=546&bookId=6754
   req.body: {userId: '546', bookId: '6754'}
   ```
   
7. **app.route(path).get(handler).post(handler)** - This syntax allows us to chain different verb handlers on the same path route. 

## Node basics -
### .env file -
The .env file is a hidden file that is used to pass environment variables to our application. This file is secret, no one but we can access it, and it can be used to store data that we want to keep private or hidden. For example, we can store API keys from external services or our database URI. We can also use it to store configuration options. By setting configuration options, we can change the behavior of our application, without the need to rewrite some code.

The environment variables are accessible from the app as `process.env.VAR_NAME`. The process.env object is a global Node object, and variables are passed as strings. By convention, the variable names are all uppercase, with words separated by an underscore. The .env is a shell file, so we don’t need to wrap names or values in quotes. It is also important to note that there cannot be space around the equals sign when you are assigning values to your variables, e.g. `VAR_NAME=value`. Usually, you will put each variable definition on a separate line.

## Sources -
1. FreeCodeCamp
