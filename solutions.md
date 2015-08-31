### Master Express.js and have fun!
─────────────────────────────────
# Exercise 1 of 8
## HELLO WORLD!

Create an Express.js app that outputs "Hello World!" when somebody goes to /home.

The port number will be provided to you by expressworks as the first argument of
the application, ie. process.argv[2].

Run $ killall node  before verifying exercises (in your terminal on Mac OS X) to end any previous processes. For Windows, use taskkill /IM node.exe.

----
### HINTS

This is how we can create an Express.js app on port 3000, that responds with
a string on '/':
```
    var express = require('express')
    var app = express()
    app.get('/', function(req, res) {
      res.end('Hello World!')
    })
    app.listen(3000)
```
Please use process.argv[2] instead of a fixed port number:
```
    app.listen(process.argv[2])
```
---
### SOLUTION
```
var express = require('express')
var app = express()
app.get('/home', function(req, res) {
  res.end('Hello World!')
})
app.listen(process.argv[2])
```

----
# Exercise 2 of 8
## STATIC

Apply static middleware to serve index.html file without any routes.

Your solution must listen on the port number supplied by process.argv[2].

The index.html file is provided and usable via the path supplied by
process.argv[3]. However, you can use your own file with this content:
```
    <html>
      <head>
        <title>expressworks</title>
        <link rel="stylesheet" type="text/css" href="/main.css"/>
      </head>
      <body>
        <p>I am red!</p>
      </body>
    </html>
```
----

## HINTS

This is how you can call static middleware:

    app.use(express.static(path.join(__dirname, 'public')));

For this exercise expressworks will pass you the path:

    app.use(express.static(process.argv[3] || path.join(__dirname, 'public')));

___

## SOLUTION
```
    var path = require('path')
    var express = require('express')
    var app = express()

    app.use(express.static(process.argv[3]||path.join(__dirname, 'public')));

    app.listen(process.argv[2])
```
---
# Exercise 3 of 8
## JADE

Create an Express.js app with a home page rendered by Jade template engine.

The homepage should respond to /home.

The view should show the current date using toDateString.

----

## HINTS

The Jade template file index.jade is already provided:

    h1 Hello World
    p Today is #{date}.

This is how to specify path in a typical Express.js app when the folder is
'templates':
```
    app.set('views', path.join(__dirname, 'templates'))
```
However, to use our index.jade, the path to index.jade will be provided as
process.argv[3].  You are welcome to use your own jade file!

To tell Express.js app what template engine to use, apply this line to the
Express.js configuration:
```
    app.set('view engine', 'jade')
```
Instead of Hello World's res.end(), the res.render() function accepts
a template name and presenter data:
```
    res.render('index', {date: new Date().toDateString()})
```
We use toDateString() to simply return the date in a human-readable format
without the time.

----

## NOTE

When creating your projects from scratch, install the jade dependency with npm.

Again, the port to use is passed by expressworks to the application as process.argv[2].

## SOLUTION
```
    var express = require('express')
    var app = express()
    app.set('view engine', 'jade')
    app.set('views', process.argv[3])
    app.get('/home', function(req, res) {
        res.render('index', {date: new Date().toDateString()})
})
    app.listen(process.argv[2])
```
# Exercise 4 of 8
## GOOD OLD FORM

Write a route ('/form') that processes HTML form input (<form><input name="str"/></form>) and prints backwards the str value.

----
## HINTS

To handle POST request use the post() method which is used the same way as get():
```
   app.post('/path', function(req, res){...})
```
Express.js uses middleware to provide extra functionality to your web server.

Simply put, a middleware is a function invoked by Express.js before your own
request handler.

Middlewares provide a large variety of functionalities such as logging, serving
static files and error handling.

A middleware is added by calling use() on the application and passing the
middleware as a parameter.

To parse x-www-form-urlencoded request bodies Express.js can use urlencoded()
middleware from the body-parser module.
```
   var bodyparser = require('body-parser')
   app.use(bodyparser.urlencoded({extended: false}))
```
Read more about Connect middleware here:
 [https://github.com/senchalabs/connect#middleware](https://github.com/senchalabs/connect#middleware)

The documentation of the body-parser module can be found here:
 [https://github.com/expressjs/body-parser](https://github.com/expressjs/body-parser)

Here is how we can flip the characters:
```
   req.body.str.split('').reverse().join('')
```
----
## NOTE

When creating your projects from scratch, install the body-parser dependency
with npm by running:
```
   $ npm install body-parser
```
…in your terminal.

Again, the port to use is passed expressworks to the application as process.argv[2].

## SOLUTION
```
var express = require('express')
var bodyParser = require('body-parser')
var app = express()

app.use(bodyParser.urlencoded({extended: false}))

app.post('/form', function(req, res) {
  res.send(req.body.str.split('').reverse().join(''))
})

app.listen(process.argv[2])

```
----

# Exercise 5 of 8
## STYLISH CSS

Style the html from the example "STATIC" using Stylus middleware. [Stylus]
([https://github.com/stylus/stylus](https://github.com/stylus/stylus)) generates .css files on-the-fly from
.styl files.

Your solution should listen on the port supplied by process.argv[2] for a
GET requests, one of which will be for main.css, which should be
automatically generated by your Stylus middleware. index.html and main.styl
can be found in process.argv[3] (they are in the same directory).

You could also create your own folder and use these, if you like:

The main.styl file:
```
    p
      color red
```
The index.html file:
```
    <html>
      <head>
        <title>expressworks</title>
        <link rel="stylesheet" type="text/css" href="/main.css"/>
      </head>
      <body>
        <p>I am red!</p>
      </body>
    </html>
```
----
## HINTS

You'll want to plug in some stylus middleware using app.use again.
It'll look something like this:
```
    app.use(require('stylus').middleware( "/path/to/*.styl" ));
```
In addition to producing in the "STATIC" exercise, you'll need to serve static files.
Remember that middleware is executed in the order app.use is called!

## NOTE

For your own projects, Stylus requires to be installed like any other
dependency:
```
    npm install stylus
```
