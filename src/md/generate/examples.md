---
title: CSV Generate Examples
description: Examples on how to generate CSV data sets and streams
keywords: ['intro','page']
sort: 1
github: 'https://github.com/adaltas/node-csv-generate'
---

## Using the callback API

The parser receive a string and return an array inside a user-provided
callback. This example is available with the command `node samples/callback.js`.

```javascript
var generate = require('csv-generate');

generate({seed: 1, columns: 2, length: 2}, function(err, output){
  output.should.eql('OMH,ONKCHhJmjadoA\nD,GeACHiN');
});
```

## Using the stream API

```javascript
// node samples/stream.js
var generate = require('csv-generate');

var data = []
var generator = generate({seed: 1, objectMode: true, columns: 2, length: 2});
generator.on('readable', function(){
  while(d = generator.read()){
    data.push(d);
  }
});
generator.on('error', function(err){
  console.info(err);
});
generator.on('end', function(){
  data.should.eql([ [ 'OMH', 'ONKCHhJmjadoA' ],[ 'D', 'GeACHiN' ] ]);
});
```

## Using the pipe function

One useful function part of the Stream API is `pipe` to interact between
multiple streams. You may use this function to pipe a `stream.Readable` string
source to a `stream.Writable` object destination.

The [pipe example](https://github.com/adaltas/node-csv-generate/blob/master/samples/pipe.js), generates a dataset of 2 rows with 2 columns. The first columns contains integer values and the second column contains boolean values. It prints the generated dataset to stdout. the function `generate` return a readable stream which is then piped to `process.stdout` which is a writable stream.

```javascript
// node samples/pipe.js
const generate = require('csv-generate')
generate({
  columns: ['int', 'bool'],
  length: 2}
)
.pipe(process.stdout)
```