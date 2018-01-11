# Node.js Cheatsheet

## Documentation
* [API Docs](http://nodejs.org/api/)
* [Debugger](http://nodejs.org/api/debugger.html)
* [Cluster](http://nodejs.org/docs/latest/api/cluster.html)
* [Up and Running with Node.js](http://ofps.oreilly.com/titles/9781449398583/)
* [Async JavaScript: Build More Responsive Apps with Less Code by Trevor Burnham](http://pragprog.com/book/tbajs/async-javascript)
* [A short guide to Connect Middleware](http://stephensugden.com/middleware_guide/)

## Installation
* [Downloadable installers](http://nodejs.org/download/)
* [Installing via various package managers](https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager)
* [Source code](https://github.com/joyent/node)

## Debugging
* [debugger](http://nodejs.org/api/debugger.html)
* [node-inspector](https://github.com/dannycoates/node-inspector)

## Utilities
* [npm](https://npmjs.org/) - package manager for Node
* [supervisor](https://github.com/isaacs/node-supervisor) - automatically restarts Node-based server when files change
* [nodemon](https://github.com/remy/nodemon) - automatically restarts Node-based server when files change
* [forever](https://github.com/nodejitsu/forever) - ensures that a script runs continuously
* [Winser](http://jfromaniello.github.com/winser/) - runs Node as a Windows service, using nssm
* [docco](http://jashkenas.github.com/docco/) and [docco-husky](https://github.com/mbrevoort/docco-husky) - documentation generator
* [node-inspector](https://github.com/dannycoates/node-inspector) - browser-based debugging interface

## Modules
These libraries are especially useful (to me, for what I have done):

* [Express](http://expressjs.com/) - web application framework
* [Connect](http://www.senchalabs.org/connect/) - middleware framework
* [Jade](http://jade-lang.com/) - template engine
* [Socket.IO](http://socket.io/) - support for WebSockets and other connection mechanisms
* [Request](https://github.com/mikeal/request) - simplified HTTP requests
* [xml2js](https://github.com/Leonidas-from-XIV/node-xml2js) - simple XML-to-JavaScript converter
* [nconf](https://github.com/flatiron/nconf) - configuration files
* [PDFKit](http://pdfkit.org/) - PDF generation
* [Backbone](http://backbonejs.org/) - client-side data models, views, UI binding
* [Underscore](http://underscorejs.org/) - functional programming helper library
* [Async.js](https://github.com/caolan/async) - asynchronous JavaScript
* [Q.js](https://github.com/kriskowal/q) - asynchronous promises
* [Mocha](http://visionmedia.github.com/mocha/) - testing framework
* [should.js](http://github.com/visionmedia/should.js) - BDD-style assertion library
* [Chai](http://chaijs.com/) - BDD- and TDD-style assertion library

A [complete list of all available modules](https://npmjs.org for).

## Creating a New Web App with the Express Framework
If you don't already have the Express framework installed, do this:
```bash
npm install -g express
```
To create a new app, do this:
```bash
express --sessions MyNewApp
cd MyNewApp
npm install
```
To run the app:
```bash
cd MyNewApp
node app
```
By default, the app will listen for requests on port 3000, so you can browse to http://locahost:3000/ to view the app. Modify app.js or set the PORT environment variable to use a different port.

## Snippets
```javascript
console.log('Hello, world!')

// Common modules
var util = require('util');
var events = require('events');
var http = require('http');
var url = require('url');
var fs = require('fs');
var zlib = require('zlib');

// Write JSON to a file
fs.writeFile('./myfile.json', JSON.stringify(obj, null, 4));

// Simple HTTP server
var s = http.createServer(function (request, response) {
  response.writeHead(200, {'Content-Type': 'text/plain'});
  response.end('Hello World\n');
});
s.listen(8124);

// Simple Express server
var express = require('express');
var app = express();
app.get('/', function(req, res){
  res.send('hello world');
});
app.listen(3000);

// socket.io server
var io = require('socket.io').listen(80);
io.sockets.on('connection', function (socket) {
  socket.emit('news', { hello: 'world' });
  socket.on('my other event', function (data) {
    console.log(data);
  });
});

// socket.io server with Express
var app = require('express')()
  , server = require('http').createServer(app)
  , io = require('socket.io').listen(server);
server.listen(80);
app.get('/', function (req, res) {
  res.sendfile(__dirname + '/index.html');
});
io.sockets.on('connection', function (socket) {
  socket.emit('news', { hello: 'world' });
  socket.on('my other event', function (data) {
    console.log(data);
  });
});

// socket.io client
<script src="/socket.io/socket.io.js"></script>
<script>
  var socket = io.connect();
  socket.on('news', function (data) {
    console.log(data);
    socket.emit('my other event', { my: 'data' });
  });
</script>

// Simple HTTP request
var request = require('request');
request('http://www.google.com', function (error, response, body) {
  if (!error && response.statusCode == 200) {
    console.log(body); // Print the google web page.
  }
});

// Parse a URL
var p = url.parse('http://undefinedvalue.com/foo/bar?baz=1');
console.log('Query: ' + p.query);

// Parse XML and display as pretty-printed JSON
var xml2js = require('xml2js');
var parser = new xml2js.Parser();
parser.parseString(myXmlData, function (error, result) {
  if (error) {
    // ...
  }
  else {
    console.log(JSON.stringify(result, null, "    ");
  }
});

// Format a string
str = util.format('Hello, %s! It is %d', 'world', 2012);

// Handle exception that bubbles up to event loop
process.on('uncaughtException', function (err) {
  console.log('Caught exception: ' + err);
});

// Schedule execution in the future
var t = setTimeout(function () {
  console.log('It is five seconds later');
}, 5000);

// Cancel a setTimeout() callback
clearTimeout(t);

// Schedule/cancel repeating timer
var interval = setInterval(function() {
  console.log("Once-per-second timer");
}, 1000);
clearInterval(interval);

// Run on next execution of event loop
process.nextTick(function () {
  console.log('nextTick callback');
});

// Create a "subclass" of EventEmitter
function MyStream() {
    events.EventEmitter.call(this);
}
util.inherits(MyStream, events.EventEmitter);
MyStream.prototype.write = function(data) {
    this.emit("data", data);
}

// Handle events on a readable stream
s.on('data', function (data) { }); // data is Buffer or string
s.on('end', function () { });
s.on('error', function (exception) { });
s.on('close', function () { });

// Handle events on a writable stream
s.on('drain', function () { });
s.on('error', function (exception) { });
s.on('close', function () { });
s.on('pipe', function (src) { });

// Compress a file
var gzip = zlib.createGzip();
var inp = fs.createReadStream('input.txt');
var out = fs.createWriteStream('input.txt.gz');
inp.pipe(gzip).pipe(out);

// Mocha tests (BDD-style)
var assert = require('assert');
describe('Test', function () {
    it('should pass', function () {
        assert.equal(1, 1);
    });
    it('should fail', function () {
        assert.equal(1, 0);
    });
});

// Mocha tests (TDD-style)
suite('Test', function () {
    test('should pass', function () {
        assert.equal(1, 1);
    });
    test('should fail', function () {
        assert.equal(1, 0);
    });
});

// should.js
var should = require('should');
should.exist({})
should.exist([])
should.exist('')
should.exist(0)
should.exist(null)      // will throw
should.exist(undefined) // will throw
should.not.exist(undefined)
should.not.exist(null)
should.not.exist('')    // will throw
should.not.exist({})    // will throw
true.should.be.ok
'yay'.should.be.ok
(1).should.be.ok
false.should.not.be.ok
''.should.not.be.ok
(0).should.not.be.ok
true.should.be.true
'1'.should.not.be.true
 false.should.be.false
 (0).should.not.be.false
var args = (function(){ return arguments; })(1,2,3);
args.should.be.arguments;
[].should.not.be.arguments;
[].should.be.empty
''.should.be.empty
({ length: 0 }).should.be.empty
({ foo: 'bar' }).should.eql({ foo: 'bar' })
[1,2,3].should.eql([1,2,3])
should.strictEqual(undefined, value)
should.strictEqual(false, value)
(4).should.equal(4)
'test'.should.equal('test')
[1,2,3].should.not.equal([1,2,3])
user.age.should.be.within(5, 50)
user.should.be.a('object')
'test'.should.be.a('string')
user.should.be.an.instanceof(User)
[].should.be.an.instanceOf(Array)
user.age.should.be.above(5)
user.age.should.not.be.above(100)
user.age.should.be.below(100)
user.age.should.not.be.below(5)
username.should.match(/^\w+$/)
user.pets.should.have.length(5)
user.pets.should.have.a.lengthOf(5)
user.should.have.property('name')
user.should.have.property('age', 15)
user.should.not.have.property('rawr')
user.should.not.have.property('age', 0)
({ foo: 'bar' }).should.have.ownProperty('foo')
res.should.have.header('content-length');
res.should.have.header('content-length', '123');
res.should.be.json
res.should.be.html
[1,2,3].should.include(3)
[1,2,3].should.not.include(4)
'foo bar baz'.should.include('baz')
'foo bar baz'.should.not.include('FOO')
[[1],[2],[3]].should.includeEql([2])
[[1],[2],[3]].should.not.includeEql([4])
(function(){
  throw new Error('fail');
}).should.throw();
(function(){
  throw new Error('fail');
}).should.throw('fail');
(function(){
  throw new Error('failed to foo');
}).should.throw(/^fail/);
var obj = { foo: 'bar', baz: 'raz' };
obj.should.have.keys('foo', 'bar');
obj.should.have.keys(['foo', 'bar']);
(1).should.eql(0, 'some useful description')
```
