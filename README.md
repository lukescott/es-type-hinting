## ES Type Hints

* [Overview and Rationale](OverviewAndRationale.md)
* [Examples](docs/examples.md)


### Abstract Operations

#### HasTypeHint(O)
----------------------
Returns if the object `O` has a type hint

1. If O does not have a \[\[TypeHint]] internal slot, return **false**
2. If O.\[\[TypeHint]] is undefined, return **false**
3. Return **true**

#### GetTypeHint(O)
----------------------
Returns the type hint of object `O`

1. Assert: HasTypeHint(O) is **true**
2. Let h be the \[\[TypeHint]] internal slot of `O`
3. Return O.\[\[TypeHint]]

### GetTypeHintDefault(H)
--------------------------
Returns the default value of a type hint

1. If *H* is **string** return **""**
2. If *H* is **number** return **0**
3. If *H* is **boolean** return **false**
4. Otherwise, return **undefined**

#### SetTypeHint(O, H)
-----------------------

Sets the type hint for object `O` to `H`

[9.1 Ordinary Object Internal Methods and Internal Slots](http://www.ecma-international.org/ecma-262/6.0/index.html#sec-ordinary-object-internal-methods-and-internal-slots)

TODO: Do we actually want to add this to every object?

Every ordinary object has an [internal slot](http://www.ecma-international.org/ecma-262/6.0/index.html#sec-object-internal-methods-and-internal-slots) called \[\[TypeHint]].
The value of this internal slot is either **undefined** or an object, and is used for reflection purposes.

[9.1.6.3 ValidateAndApplyPropertyDescriptor (O, P, extensible, Desc, current)](http://www.ecma-international.org/ecma-262/6.0/index.html#sec-validateandapplypropertydescriptor)

TODO. Need help from someone who's more familiar than me with this. I'm not even sure this is the correct place to have this logic.
```javascript
let O = { n number, s string, b boolean, x any, u UserDefined };
// Should desugar to something like:
let O = { x: 0, s: "", b: false, x: undefined, u: undefined };
```

1. if Desc.\[\[Value]] is **undefined** and HasTypeHint(P)
   a. let H = GetTypeHint(P)
   b. Set *Desc*.\[\[Value]] to GetTypeHintDefault(H)

[9.1.x \[\[TypeHint]]](http://www.ecma-international.org/ecma-262/6.0/index.html#sec-ordinary-object-internal-methods-and-internal-slots)

TODO: ...

[9.2 ECMAScript Function Objects](http://www.ecma-international.org/ecma-262/6.0/index.html#sec-ecmascript-function-objects)

Table 27 &mdash; Internal Slots of ECMAScript Function Objects
| Internal | Type | Description
| \[\[TypeHint]] | Object | The return type of the function if specified, otherwise defaults to **any**

[9.2.4 FunctionInitialize  (F, kind, ParameterList, Body, Scope)](http://www.ecma-international.org/ecma-262/6.0/index.html#sec-functioninitialize)

TODO: description update.

TODO: insert steps before final

15\. if *typeHint* is not present, let *typeHint* be **any**
16\. Set the \[\[TypeHint]] [internal slot]() of *F* to *typeHint*.
17\. Return *F*

[9.2.5 FunctionCreate (kind, ParameterList, Body, Scope, Strict, prototype [,TypeHint])](http://www.ecma-international.org/ecma-262/6.0/index.html#sec-functioncreate)

TODO: description update
TODO: full steps

5\. Return [FunctionInitialize(*F*, *kind*, *ParameterList*, *Body*, *Scope*, *TypeHint*)](#sec-functioninitialize)

[9.2.6 GeneratorFunctionCreate (kind, ParameterList, Body, Scope, Strict [,TypeHint])](http://www.ecma-international.org/ecma-262/6.0/index.html#sec-generatorfunctioncreate)

TODO: description udpate
TODO: full steps

5\. Return [FunctionInitialize(*F*, *kind*, *ParameterList*, *Body*, *Scope*, *TypeHint*)](#sec-functioninitialize)


[9.2.12 FunctionDeclarationInstantiation(func, argumentsList)](http://www.ecma-international.org/ecma-262/6.0/index.html#sec-functiondeclarationinstantiation)

TODO: description update?

TODO: full steps.

TODO: insert after 28.f.i.a

28.f.i.b. If *initialValue* is **undefined** and HasTypeHint(n)
28.f.i.b.i   Let H be GetTypeHint(n)
28.f.i.b.ii  Set *initialValue* to GetTypeHintDefault(H)


### Syntax ###
  [TODO TypeHint](http://www.ecma-international.org/ecma-262/6.0/)
  
  ```
  TypeHint[Yield] :
    [~Yield] IdentifierReference[~Yield]
    number
    string
    boolean
    void
  ```

  [12.2.6 Object Initializer](http://www.ecma-international.org/ecma-262/6.0/#sec-object-initializer)
  
  ```
  PropertyDefinition[Yield] :
    IdentifierReference[?Yield]
    CoverInitializedName[?Yield]
    PropertyName[?Yield] TypeHint[?Yield]opt  : AssignmentExpression[In, ?Yield]
    MethodDefinition[?Yield]
    
  CoverInitializedName[Yield] :
    IdentifierReference[?Yield] TypeHint[?Yield]opt Initializer[In, ?Yield]
  
  ```
  
  [12.2.6.x Static Semantics](http://www.ecma-international.org/ecma-262/6.0/#sec-object-initializer)
  
  *PropertyDefinition* : *PropertyName* *TypeHint*  **:** *AssignmentExpression*
  
  1. SetTypeHint(PropertyName, TypeHint)
  
  
  [12.2.6.9 Runtime Semantics: PropertyDefinitionEvaluation](http://www.ecma-international.org/ecma-262/6.0/index.html#sec-object-initializer-runtime-semantics-propertydefinitionevaluation)
  
  *PropertyDefinition* **:** *IdentifierReference* *TypeHint*
  
  
  TODO: do I need "Let propValue be GetValue(exprValue)." ?
  1. Let *propName* be StringValue of *IdentifierReference*
  2. Let *exprValue* be GetTypeHintDefault(*TypeHint*)
  3. ReturnIfAbrupt(*exprValue*)
  4. Assert: *enumerable* is **true**
  5. Return CreateDataPropertyOrThrow(*object*, *propName*, *exprValue*)

  [12.14.5 Destructuring Assignment](http://www.ecma-international.org/ecma-262/6.0/#sec-destructuring-assignment)
  ```
  AssignmentProperty[Yield] :
    IdentifierReference[?Yield] TypeHint[?Yield]opt Initializer[In,?Yield]opt
    PropertyName TypeHint[?Yield] : AssignmentElement[?Yield]
  ```
  
  [13.3.1 Let and Const Declarations](http://www.ecma-international.org/ecma-262/6.0/#sec-let-and-const-declarations)
  ```
  LexicalBinding[In, Yield] :
    BindingIdentifier[?Yield] TypeHint[?Yield]opt Initializer[?In, ?Yield]opt
    BindingPattern[?Yield] Initializer[?In, ?Yield]
  ```
  
  [13.3.2 Variable Statement](http://www.ecma-international.org/ecma-262/6.0/#sec-variable-statement)
  ```
  VariableDeclaration[In, Yield] :
    BindingIdentifier[?Yield] TypeHint[?Yield]opt Initializer[?In, ?Yield]opt
    BindingPattern[?Yield] Initializer[?In, ?Yield]
  ```
  
  [13.3.3 Destructuring Binding Patterns](http://www.ecma-international.org/ecma-262/6.0/#sec-destructuring-binding-patterns)
  ```
  SingleNameBinding [Yield, GeneratorParameter] :
    [+GeneratorParameter] BindingIdentifier[Yield] TypeHint[?Yield]opt Initializer[In]opt
    [~GeneratorParameter] BindingIdentifier[?Yield] TypeHint[?Yield]opt Initializer[In, ?Yield]opt
  ```
  [13.7 Iteration Statements](http://www.ecma-international.org/ecma-262/6.0/#sec-iteration-statements)
  ```
  ForBinding[Yield] :
    BindingIdentifier[?Yield] TypeHint[?Yield]opt
    BindingPattern[?Yield]
  ```
  
  [14.1 Function Definitions](http://www.ecma-international.org/ecma-262/6.0/#sec-function-definitions)
  ```
  FunctionDeclaration[Yield, Default] :
    function BindingIdentifier[?Yield] ( FormalParameters ) TypeHint[?Yield]opt { FunctionBody }
    [+Default] function ( FormalParameters ) TypeHint[?Yield]opt { FunctionBody }
  FunctionExpression :
    function BindingIdentifieropt ( FormalParameters ) TypeHint[?Yield]opt { FunctionBody }
  
  FormalParameter[Yield,GeneratorParameter] :
    BindingElement[?Yield, ?GeneratorParameter] TypeHint[?Yield]opt
  ```
  
  [14.2 Arrow Function Definitions](http://www.ecma-international.org/ecma-262/6.0/#sec-arrow-function-definitions)
  ```
  ArrowParameters[Yield] :
    BindingIdentifier[?Yield] TypeHint[?Yield]opt
    CoverParenthesizedExpressionAndArrowParameterList[?Yield] TypeHint[?Yield]opt
    
  ```
  
  TODO: semantics of what `TypeHint` is attached to.
  
  [14.3 Method Definitions](http://www.ecma-international.org/ecma-262/6.0/#sec-method-definitions)
  ```
  MethodDefinition[Yield] :
    PropertyName[?Yield] ( StrictFormalParameters ) TypeHint[?Yield]opt { FunctionBody }
    GeneratorMethod[?Yield]
    get PropertyName[?Yield] ( ) TypeHint[?Yield]opt { FunctionBody }
    set PropertyName[?Yield] ( PropertySetParameterList ) TypeHint[?Yield]opt { FunctionBody }
  ```
  TODO: static semantics of set's typehint, it should be a syntax error if it's not empty, or "void"
  
  [14.4 Generator Function Definitions](http://www.ecma-international.org/ecma-262/6.0/#sec-generator-function-definitions)
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
