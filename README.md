# sizedown

A LevelDOWN that monitors and optionally limits the on-disk size.

[![Travis](http://img.shields.io/travis/karissa/sizedown.svg?style=flat)](https://travis-ci.org/karissa/sizedown)

```
npm install sizedown
```

### `db = sizedown([leveldown], [bytes])`

## Example

Monitor the size and see the current size in bytes with `db.db.getSize`:

```js
var sizedown = require('sizedown')

function down (loc) {
  return sizedown(memdown(loc), 0)
}

var db = levelup('test', {db: down)})

db.put('akey', 'a value', function (err) {
  db.db.getSize(function (err, size) {
    // size === 7 or length of 'a value'
  })
})
```


```js

function down (bytes) {
  // number of bytes to allow via put and batch before erroring
  return sizedown(memdown(), {limit: bytes})
}

var db = levelup('hello', down(1))

db.on('ready', function () {
  db.put('akey', 'a value', function (err) {
    // err.message = 'Exceeds limit of 1 byte'
    db.get('akey', function (err, value) {
      // err.notFound === true
    })
  })
})
```
