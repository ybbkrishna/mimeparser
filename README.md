# mimeparser

Lib for parsing mime streams.

## Scope

This is supposed to be a "low level" mime parsing module. No magic is performed on the data (eg. no joining HTML parts etc.). All body data is emitted out as ArrayBuffer values, so no need to perform any base64 or quoted printable decoding by yourself.

[![Build Status](https://travis-ci.org/whiteout-io/mimeparser.png?branch=master)](https://travis-ci.org/whiteout-io/mimeparser)

## Installation

### [volo](http://volojs.org/):

    volo add whiteout-io/mimeparser/0.1.0

### [Bower](http://bower.io/):

    bower install git@github.com:whiteout-io/mimeparser.git#0.1.0

### [npm](https://www.npmjs.org/):

    npm install https://github.com/whiteout-io/mimeparser/tarball/0.1.0

## Dependencies

This module depends on [mimefuncs](https://github.com/whiteout-io/mimefuncs). The dependency will not be fetched automatically, so make sure it is there.

## Usage

### AMD

    var MimeParser = require('mimeparser');

### non-AMD

    <script src="mimeparser"></script>
    // exposes MimeParser the constructor to the global object

### Feed data to the parser

Feed data with `write(chunk)`. Where `chunk` is supposed to be an ArrayBuffer or a "binary" string.

```javascript
parser.write("Subject: test\n\nHello world!");
```

When all data is feeded to the parser, call `end()`

```javascript
parser.end();
```

### Receiveing the output

You can receive the output by creating appropriate event handler functions.

#### Headers

To receive node headers, define `onheader` function

```javascript
parser.onheader = function(node){
    console.log(node.header.join("\n")); // List all headers
    console.log(node.headers["content-type"]); // List value for Content-Type
};
```

#### Body

Body is emitted in chunks of ArrayBuffers, define `onbody` to catch these chunks

```javascript
parser.onbody = function(node, chunk){
    console.log("Received " + chunk.byteLength + " bytes for " + node.path.join("."));
};
```

#### Parse end

When the parsing is finished, `onend` is called

```javascript
parser.onend = function(){
    console.log("Parsing is finished");
};
```

## Quirks

This seems like asynchronous but actually it is not. So always define `onheader`, `onbody` and `onend` before writing the first chunk of data to the parser.

**message/rfc822** is automatically parsed if the mime part does not have a `Content-Disposition: attachment` header, otherwise it will be emitted as a regular attachment (as one long ArrayBuffer value).

## Hands on

```bash
git clone git@github.com:whiteout-io/mimeparser.git
cd mimeparser
npm install && npm test
```

## License

    Copyright (c) 2013 Andris Reinman

    Permission is hereby granted, free of charge, to any person obtaining a copy
    of this software and associated documentation files (the "Software"), to deal
    in the Software without restriction, including without limitation the rights
    to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
    copies of the Software, and to permit persons to whom the Software is
    furnished to do so, subject to the following conditions:

    THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
    IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
    FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
    AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
    LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
    OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
    SOFTWARE.