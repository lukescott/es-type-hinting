## ECMAScript Type Hinting ##

### Overview

The goal of this proposal is to provide a syntactic mechanism for type hinting in ECMAScript without specifying how those type hints are to be used. With inspiration from Go, it propses that type hints are placed after the thing they are modifying, but using only white-space (SP / HTAB) as a separator. This is different to most existing proposals and implementations which use a colon `:` as a separator.


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

### Motivation ###

A lot of variations of type hinting exists, such as flow, TypeScript, and other proposals. Most of these syntaxes use a colon `:`, similar to ActionScript, and also existed in the abandoned [ECMAScript 4](http://www.ecmascript.org/es4/spec/overview.pdf) spec. The `:` syntax is ambiguous to existing plain JavaScript objects, and ES6 destructuring:

```javascript
var point = {
  x: 0,
  y: 0
};

var point = {
  x: Number, // These aren't type annotations, but rather, is a map of x => Number, y => Number
  y: Number
};
// Where would the type hint go in destructuring? 
let {x: xcoord = defaultValue} = point;

```
This proposal introduces a simple way of doing type hinting without complicated syntax:

```javascript
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

// this
let { x: y Number = 0 } = o;
// instead of
let { x:Number: y = 0 } = o;
// or
let { x: y:Number = 0 } = o;
```

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

### Rationale

*Why is the type hint after the thing it modifies, instead of before like in C++?*

The biggest reason for this decision is that it allows all declarations to start with a keyword, otherwise you end up with inconsistant syntax:
```javascript
// You have:
Number function a() { return 1; }
Number let b = 1;
// But also have:
class C {}
```
Requiring the hint to go after keeps the keyword first on the line.

The second reason is precedent. The previous incarnation of adding typing to the language occured in [ECMAScript 4](http://www.ecmascript.org/es4/spec/overview.pdf), which added the type in the same position as ActionScript, and is now the chosen position in related languages such as TypeScript and Flow. There is also an argument to be made when relating to natural languages: the ordering of "noun adjective" (as in French, and similar to this proposal) is [twice as common](http://wals.info/feature/87A#2/18.0/144.8) as the "adjective noun" ordering (as in English, and similar to C++). 

*Why use a space as a separator instead of a colon `:` like in TypeScript?*

The primary reason, as mentioned in the Motivation section, is that using the colon becomes awkward in certain cases, especially with object destructuring.
```javascript
let { name:alias } = o; // Where does the ": TypeHint" go?
```

The second reason is precedent. Go-lang is the primary precendent to use spaces as the annotation separator.


### Syntax ###
  [TODO TypeHint](https://people.mozilla.org/~jorendorff/es6-draft.html)
  
  ```
  TypeHint[Yield] :
    [~Yield] IdentifierReference[~Yield]
    number
    string
    boolean
    void
  ```


  [12.2.5 Object Initializer](https://people.mozilla.org/~jorendorff/es6-draft.html#sec-object-initializer)
  
  ```
  PropertyDefinition[Yield] :
    IdentifierReference[?Yield]
    CoverInitializedName[?Yield]
    PropertyName[?Yield] TypeHint  : AssignmentExpression[In, ?Yield]
    MethodDefinition[?Yield]
    
  CoverInitializedName[Yield] :
    IdentifierReference[?Yield] TypeHint Initializer[In, ?Yield]
  
  
  ```
  [12.14.5 Destructuring Assignment](https://people.mozilla.org/~jorendorff/es6-draft.html#sec-destructuring-assignment)
  ```
  AssignmentProperty[Yield] :
    IdentifierReference[?Yield] TypeHint Initializer[In,?Yield]opt
    PropertyName TypeHint : AssignmentElement[?Yield]
  ```
  
  [13.2.1 Let and Const Declarations](https://people.mozilla.org/~jorendorff/es6-draft.html#sec-let-and-const-declarations)
  ```
  LexicalBinding[In, Yield] :
    BindingIdentifier[?Yield] TypeHint Initializer[?In, ?Yield]opt
    BindingPattern[?Yield] Initializer[?In, ?Yield]
  ```
  
  [13.2.2 Variable Statement](https://people.mozilla.org/~jorendorff/es6-draft.html#sec-variable-statement)
  ```
  VariableDeclaration[In, Yield] :
    BindingIdentifier[?Yield] TypeHint Initializer[?In, ?Yield]opt
    BindingPattern[?Yield] Initializer[?In, ?Yield]
  ```
  
  [13.2.3 Destructuring Binding Patterns](https://people.mozilla.org/~jorendorff/es6-draft.html#sec-destructuring-binding-patterns)
  ```
  SingleNameBinding [Yield, GeneratorParameter] :
    [+GeneratorParameter] BindingIdentifier[Yield] TypeHint Initializer[In]opt
    [~GeneratorParameter] BindingIdentifier[?Yield] TypeHint Initializer[In, ?Yield]opt
  ```
  [13.6 Iteration Statements](https://people.mozilla.org/~jorendorff/es6-draft.html#sec-iteration-statements)
  ```
  ForBinding[Yield] :
    BindingIdentifier[?Yield] TypeHint
    BindingPattern[?Yield]
  ```
  
  [14.1 Function Definitions](https://people.mozilla.org/~jorendorff/es6-draft.html#sec-function-definitions)
  ```
  FunctionDeclaration[Yield, Default] :
    function BindingIdentifier[?Yield] ( FormalParameters ) TypeHint { FunctionBody }
    [+Default] function ( FormalParameters ) TypeHint { FunctionBody }
  FunctionExpression :
    function BindingIdentifieropt ( FormalParameters ) TypeHint { FunctionBody }
  
  FormalParameter[Yield,GeneratorParameter] :
    BindingElement[?Yield, ?GeneratorParameter] TypeHint
  ```
  
  [14.2 Arrow Function Definitions](https://people.mozilla.org/~jorendorff/es6-draft.html#sec-arrow-function-definitions)
  ```
  ArrowParameters[Yield] :
    BindingIdentifier[?Yield] TypeHint
    CoverParenthesizedExpressionAndArrowParameterList[?Yield] TypeHint
    
  ```
  
  TODO: semantics of what `TypeHint` is attached to.
  
  [14.3 Method Definitions](https://people.mozilla.org/~jorendorff/es6-draft.html#sec-method-definitions)
  ```
  MethodDefinition[Yield] :
    PropertyName[?Yield] ( StrictFormalParameters ) TypeHint { FunctionBody }
    GeneratorMethod[?Yield]
    get PropertyName[?Yield] ( ) TypeHint { FunctionBody }
    set PropertyName[?Yield] ( PropertySetParameterList ) { FunctionBody }
  ```
  TODO: does `set` need a TypeHint? Probably not...
  
  [14.4 Generator Function Definitions](https://people.mozilla.org/~jorendorff/es6-draft.html#sec-generator-function-definitions)
  ```
  GeneratorMethod[Yield] :
    * PropertyName[?Yield] ( StrictFormalParameters[Yield,GeneratorParameter] ) TypeHint { GeneratorBody }
  GeneratorDeclaration[Yield, Default] :
    function * BindingIdentifier[?Yield] ( FormalParameters[Yield,GeneratorParameter] ) TypeHint { GeneratorBody }
    [+Default] function * ( FormalParameters[Yield,GeneratorParameter] ) TypeHint { GeneratorBody }
  GeneratorExpression :
    function * BindingIdentifier[Yield]opt ( FormalParameters[Yield,GeneratorParameter] ) TypeHint { GeneratorBody }
  ```

### Prior Art:
Type Hinting:
* Closure Compiler
* JSDoc
* [Flow Comments](http://flowtype.org/blog/2015/02/20/Flow-Comments.html)
 
Type Checking/Annotations:
* Flow
* TypeScript
* ActionScript
* Closure Compiler
* [Abandoned ECMAScript 4](http://www.ecmascript.org/es4/spec/overview.pdf)
