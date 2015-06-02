## ECMAScript Type Hinting ##

This proposal introduces type hinting.

Inspired by types in Go. Similar to flow or TypeScript, but uses WSP (SP / HTAB) ` ` instead of a colon `:`.

### Examples ###

```javascript
// without defaults
// uses type default: Number = 0, String = "", [Object] = undefined
var point = {
  x Number,
  y Number
};

// with defaults
var point2 = {
  x Number: 10,
  y Number: 50
};

// without defaults, same as var point above
class Point {
  x Number,
  y Number
}

// with defaults, assuming properties use ':'
class Point2 {
  x Number: 0
  y Number: 0
}

// with defaults, assuming properties use '='
class Point3 {
  x Number = 0
  y Number = 0
}

function print(text String) {
  console.log(text);
}

function toLowerCase(text String) String {
  return text.toLowerCase();
}

function map(array Array, callback Function) {
  return array.map(callback);
}
```

### Motivation and Overview ###

A lot of variations of type hinting exists, such as flow, TypeScript, and other proposals. These syntaxes use a colon `:`, similar to ActionScript. The `:` syntax is ambiguous to existing plain JavaScript objects:

```javascript
var point = {
  x: 0,
  y: 0
};
// vs
var point = {
  x: Number,
  y: Number
};
```

The motivation behind this proposal is to introduce a simple way of doing type hinting without complicated syntax.

``javascript
// this
var point = {
  x Number: 0,
  y Number: 0
};
// instead of (requires `= value`)
var point = {
  x: Number = 0,
  y: Number = 0
};
// or some other complex syntaxs
var point = {
  x :Number: 0,
  y :Number: 0
};
var point = {
  x :: Number : 0,
  y :: Number : 0
};
``

Using simple spaces translates well in other areas:
```javascript
function map(array Array, callback Function) {...}

// doesn't matter if class properties end up using `:` or `=`
class Point {
  x Number,
  y Number
}
class Point2 {
  x Number: 0
  y Number: 0
}
class Point3 {
  x Number = 0
  y Number = 0
}
```

### Syntax ###

TODO
