## ES Type Hints

* [Overview and Rationale](OverviewAndRationale.md)
* [Examples](docs/examples.md)

### Syntax ###
  [TODO TypeHint](http://www.ecma-international.org/ecma-262/6.0/)
  
  ```
  TypeHint[Yield, ReturnType] :
    [~Yield] IdentifierReference[~Yield]
    number
    string
    boolean
    [+ReturnType] void
    any
  ```

  [12.2.6 Object Initializer](http://www.ecma-international.org/ecma-262/6.0/#sec-object-initializer)
  
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
    function BindingIdentifier[?Yield] ( FormalParameters ) TypeHint[?Yield, ReturnType]opt { FunctionBody }
    [+Default] function ( FormalParameters ) TypeHint[?Yield]opt { FunctionBody }
  FunctionExpression :
    function BindingIdentifieropt ( FormalParameters ) TypeHint[?Yield, ReturnType]opt { FunctionBody }
  
  FormalParameter[Yield,GeneratorParameter] :
    BindingElement[?Yield, ?GeneratorParameter] TypeHint[?Yield]opt
  ```
  
  [14.2 Arrow Function Definitions](http://www.ecma-international.org/ecma-262/6.0/#sec-arrow-function-definitions)
  ```
  ArrowParameters[Yield] :
    BindingIdentifier[?Yield] TypeHint[?Yield]opt
    CoverParenthesizedExpressionAndArrowParameterList[?Yield] TypeHint[?Yield, ReturnType]opt
    
  ```
  
  [14.2.x Semantics](https://people.mozilla.org/~jorendorff/es6-draft.html#sec-arrow-function-definitions)
    TODO: wording, title wording
    
    *ArrowParameters* **:** *BindingIdentifier* *TypeHint*
    
      * *TypeHint* is type associated with the singlar parameter specified by *BindingIdentifier*
    
    *ArrowParameters* **:** *CoverParenthesizedExpressionAndArrowParameterList* *TypeHint*
    
      * *TypeHint* is the type associated with the return type of the arrow function.
  
  [14.3 Method Definitions](http://www.ecma-international.org/ecma-262/6.0/#sec-method-definitions)
  ```
  MethodDefinition[Yield] :
    PropertyName[?Yield] ( StrictFormalParameters ) TypeHint[?Yield]opt { FunctionBody }
    GeneratorMethod[?Yield]
    get PropertyName[?Yield] ( ) TypeHint[?Yield]opt { FunctionBody }
    set PropertyName[?Yield] ( PropertySetParameterList ) TypeHint[?Yield]opt { FunctionBody }
  ```
  [14.3.1 Static Semantics: Early Errors](https://people.mozilla.org/~jorendorff/es6-draft.html#sec-method-definitions-static-semantics-early-errors)
  
    
    *MethodDefinition* **:** **set** *PropertyName* **(** *PropertySetParameterList* **)** *TypeHint* **{** *FunctionBody* **}**
    
    * It is a Syntax Error if BoundNames of *PropertySetParameterList* contains any duplicate elements.
    * It is a Syntax Error if any element of the BoundNames of *PropertySetParameterList* also occurs in the LexicallyDeclaredNames of *FunctionBody*.
    * It is a Syntax Error if *TypeHint* is anything other than **void**
  
  
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
