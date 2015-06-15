
### Examples ###

Objects:
```javascript
var point = {
  x number, // defaults to 0
  y number: 0 // provide value
};
```

ES6 destructuring:
```javascript
let {x: xcoord number = defaultValue} = point;
```

ES6 Classes:
```javascript
class Point {
  x number // defaults to 0
  y number: 0 // provide value
  
  move(x number, y number) {
    // ...
  }
  
  distanceFrom(x number, y number) number {
    let distance number; // default to 0
    // ...
    return distance
  }
}
```

Functions:
```javascript
// text defaults to ""
function exclaim(text string) string {
  return text + "!";
}

exclaim("hello"); // "hello!"
exclaim(); // "!" not "undefined!"
```
```javascript
class Foo {}

// foo and callback don't have defaults
function callbackFoo(foo Foo, callback Function) {
  callbackFoo(foo);
}
```
