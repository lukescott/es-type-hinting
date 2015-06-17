## ECMAScript Type Hinting ##

[![Join the chat at https://gitter.im/lukescott/es-type-hinting](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/lukescott/es-type-hinting?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

### Overview

The goal of this proposal is to provide a syntactic mechanism for type hinting in ECMAScript without specifying how those type hints are to be used. With inspiration from Go, it propses that type hints are placed after the thing they are modifying, but using only white-space (SP / HTAB) as a separator. This is different to most existing proposals and implementations which use a colon `:` as a separator.

### Motivation ###

Inconsistent types lead to common bugs in JavaScript programs. Some common examples:

```javascript
"5" + 5 // "55"
"5" + undefined // "5undefined"
5 + undefined // NaN
```

Specifying types and sticking to them is a must. Facebook, who maintains
9.9+ million lines of code, developed a solution to this problem called
[flow](http://flowtype.org/). There have also been other proposals, notably
an abandoned [ECMAScript 4](http://www.ecmascript.org/es4/spec/overview.pdf)
spec.

There is also [TypeScript](http://www.typescriptlang.org/), a strict superset
of JavaScript, that provides optional static typing. Angular 2.0 is
[currently using TypeScript](http://blogs.msdn.com/b/typescript/archive/2015/03/05/angular-2-0-built-on-typescript.aspx).

All of the above solutions are great, but suffer from the following issues:

- They are not part of the ECMA standard.
- They use a colon `:` which is ambiguous with:
    - JavaScript objects
    ```javascript
    var point = {
      x: 0,
      y: 0
    };
    ```
    - ES6 destructuring
    ```javascript
    let {x: xcoord = defaultValue} = point;
    ```

The goal of this proposal is to introduce a simple syntax for type hinting in JavaScript that is compatible with existing features.

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

### Rationale

*Why is the type hint after the thing it modifies, instead of before like in C++?*

The biggest reason for this decision is that it allows all declarations to start with a keyword, otherwise you end up with inconsistant syntax:
```javascript
// You have:
number function a() { return 1; }
number let b = 1;
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
    PropertyName[?Yield] TypeHint[?Yield]opt  : AssignmentExpression[In, ?Yield]
    MethodDefinition[?Yield]
    
  CoverInitializedName[Yield] :
    IdentifierReference[?Yield] TypeHint[?Yield]opt Initializer[In, ?Yield]
  
  
  ```
  [12.14.5 Destructuring Assignment](https://people.mozilla.org/~jorendorff/es6-draft.html#sec-destructuring-assignment)
  ```
  AssignmentProperty[Yield] :
    IdentifierReference[?Yield] TypeHint[?Yield]opt Initializer[In,?Yield]opt
    PropertyName TypeHint[?Yield] : AssignmentElement[?Yield]
  ```
  
  [13.2.1 Let and Const Declarations](https://people.mozilla.org/~jorendorff/es6-draft.html#sec-let-and-const-declarations)
  ```
  LexicalBinding[In, Yield] :
    BindingIdentifier[?Yield] TypeHint[?Yield]opt Initializer[?In, ?Yield]opt
    BindingPattern[?Yield] Initializer[?In, ?Yield]
  ```
  
  [13.2.2 Variable Statement](https://people.mozilla.org/~jorendorff/es6-draft.html#sec-variable-statement)
  ```
  VariableDeclaration[In, Yield] :
    BindingIdentifier[?Yield] TypeHint[?Yield]opt Initializer[?In, ?Yield]opt
    BindingPattern[?Yield] Initializer[?In, ?Yield]
  ```
  
  [13.2.3 Destructuring Binding Patterns](https://people.mozilla.org/~jorendorff/es6-draft.html#sec-destructuring-binding-patterns)
  ```
  SingleNameBinding [Yield, GeneratorParameter] :
    [+GeneratorParameter] BindingIdentifier[Yield] TypeHint[?Yield]opt Initializer[In]opt
    [~GeneratorParameter] BindingIdentifier[?Yield] TypeHint[?Yield]opt Initializer[In, ?Yield]opt
  ```
  [13.6 Iteration Statements](https://people.mozilla.org/~jorendorff/es6-draft.html#sec-iteration-statements)
  ```
  ForBinding[Yield] :
    BindingIdentifier[?Yield] TypeHint[?Yield]opt
    BindingPattern[?Yield]
  ```
  
  [14.1 Function Definitions](https://people.mozilla.org/~jorendorff/es6-draft.html#sec-function-definitions)
  ```
  FunctionDeclaration[Yield, Default] :
    function BindingIdentifier[?Yield] ( FormalParameters ) TypeHint[?Yield]opt { FunctionBody }
    [+Default] function ( FormalParameters ) TypeHint[?Yield]opt { FunctionBody }
  FunctionExpression :
    function BindingIdentifieropt ( FormalParameters ) TypeHint[?Yield]opt { FunctionBody }
  
  FormalParameter[Yield,GeneratorParameter] :
    BindingElement[?Yield, ?GeneratorParameter] TypeHint[?Yield]opt
  ```
  
  [14.2 Arrow Function Definitions](https://people.mozilla.org/~jorendorff/es6-draft.html#sec-arrow-function-definitions)
  ```
  ArrowParameters[Yield] :
    BindingIdentifier[?Yield] TypeHint[?Yield]opt
    CoverParenthesizedExpressionAndArrowParameterList[?Yield] TypeHint[?Yield]opt
    
  ```
  
  TODO: semantics of what `TypeHint` is attached to.
  
  [14.3 Method Definitions](https://people.mozilla.org/~jorendorff/es6-draft.html#sec-method-definitions)
  ```
  MethodDefinition[Yield] :
    PropertyName[?Yield] ( StrictFormalParameters ) TypeHint[?Yield]opt { FunctionBody }
    GeneratorMethod[?Yield]
    get PropertyName[?Yield] ( ) TypeHint[?Yield]opt { FunctionBody }
    set PropertyName[?Yield] ( PropertySetParameterList ) TypeHint[?Yield]opt { FunctionBody }
  ```
  TODO: static semantics of set's typehint, it should be a syntax error if it's not empty, or "void"
  
  [14.4 Generator Function Definitions](https://people.mozilla.org/~jorendorff/es6-draft.html#sec-generator-function-definitions)
  ```
  GeneratorMethod[Yield] :
    * PropertyName[?Yield] ( StrictFormalParameters[Yield,GeneratorParameter] ) TypeHint[?Yield]opt { GeneratorBody }
  GeneratorDeclaration[Yield, Default] :
    function * BindingIdentifier[?Yield] ( FormalParameters[Yield,GeneratorParameter] ) TypeHint[?Yield]opt { GeneratorBody }
    [+Default] function * ( FormalParameters[Yield,GeneratorParameter] ) TypeHint[?Yield]opt { GeneratorBody }
  GeneratorExpression :
    function * BindingIdentifier[Yield]opt ( FormalParameters[Yield,GeneratorParameter] ) TypeHint[?Yield]opt { GeneratorBody }
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
