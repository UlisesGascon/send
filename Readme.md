
# send

  Better streaming static file server for node with Range and conditional-GET support.

## Installation

    $ npm install send

## Examples

  Small:

```js
var http = require('http');
var send = require('send');

var app = http.createServer(function(req, res){
  send(req.url).pipe(res);
});
```

  Serving from a root directory with custom error-handling:

```js
var http = require('http');
var send = require('send');

var app = http.createServer(function(req, res){
  // your custom error-handling logic:
  function error(err) {
    res.statusCode = err.status || 500;
    res.end(err.message);
  }

  // your custom directory handling logic:
  function redirect() {
    res.statusCode = 301;
    res.setHeader('Location', req.url + '/');
    res.end('Redirecting to ' + req.url + '/');
  }

  // transfer arbitrary files from within
  // /www/example.com/public/*
  send(url.parse(req.url).pathname)
  .root('/www/example.com/public')
  .on('error', error)
  .on('directory', redirect)
  .pipe(res);
});
```

## Events

  - `error` an error occurred `(err)`
  - `directory` a directory was requested
  - `stream` file streaming has started `(stream)`
  - `end` streaming has completed

## Error-handling

  By default when no `error` listeners are present an automatic response will be made, otherwise you have full control over the response, aka you may show a 5xx page etc.

## About

  Send is Connect's `static()` extracted for generalized use, a secure file
  server supporting partial responses (Ranges), conditional-GET negotiation, high test coverage, and emits
  detailed errors which may be leveraged to take appropriate actions in your application or framework.

  It does _not_ perform internal caching, you should use a reverse proxy cache such
  as Varnish for this, or those fancy things called CDNs. If your application is small enough that it would benefit from single-node memory caching, it's small enough that it does not need caching at all ;).

## Debugging

 To enable `debug()` instrumentation output export __DEBUG__:

```
$ DEBUG=send node app
```

## Running tests

```
$ npm install
$ make test
```

## License 

(The MIT License)

Copyright (c) 2012 TJ Holowaychuk &lt;tj@vision-media.ca&gt;

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.