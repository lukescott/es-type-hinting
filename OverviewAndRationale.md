## ECMAScript Type Hinting ##

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


