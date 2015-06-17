
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
    IdentifierReference[?Yield] TypeHint[?Yield]opt
    CoverInitializedName[?Yield]
    PropertyName[?Yield] : AssignmentExpression[In, ?Yield]
    MethodDefinition[?Yield]
  
  PropertyName[Yield,GeneratorParameter] :
    LiteralPropertyName
    [+GeneratorParameter] ComputedPropertyName TypeHint[?Yield]opt
    [~GeneratorParameter] ComputedPropertyName[?Yield] TypeHint[?Yield]opt
  
  LiteralPropertyName :
    IdentifierName TypeHintopt
    StringLiteral
    NumericLiteral
  
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
