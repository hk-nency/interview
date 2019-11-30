# Javascript Deep Dive

This document is written to help JavaScript developers to understand JavaScript's weird parts deeply and to prepare for interviews, the following resources was really helpful to write this document:
* [ECMAScript 6 Language specification](http://www.ecma-international.org/ecma-262/6.0/index.html)
* [ECMAScript 5 Language specification](http://es5.github.io/#x10.3)
* [MDN](https://developer.mozilla.org/bm/docs/Web/JavaScript)
* [The Modern JavaScript Tutorial](https://javascript.info)

# General Notes

* No attribute is required for the `script` element.
* The benefit of a separate file is browser caching.
* If `src` attribute is set, the content is ignored.
* JavaScript interprets the line break as an **implicit** semicolon (not always).

# Concepts And Terms

### Execution Context
  * Active execution contexts logically form the call stack.
  * The top execution context on the call stack is the running execution context.
  * The interpreter starts by the global execution context.
  * Each invocation of a function, creates a new execution context appended to the top of the execution stack.
  * Low level explanation
    * Breaks into:
      * LexicalEnvironment
        * Is a lexical environment.
        * Holds `let` and `const` declarations.
        * `Outer` reference.
      * VariableEnvironment.
        * Is a lexical environment.
        * Holds bindings created by `VariableStatements` and `FunctionDeclarations`.
        * `Outer` reference.
      * ThisBinding
        * The value associated with the `this` keyword.
  * High level explanation
    * Can be devided to
      * Creation phase
        * Creates a variable object:
          * Variables declarations.
          * Function declarations.
          * Arguments.
        * Create the scope chain.
        * Determine the value of `this`.
      * Execution phase:
        * The code is interpreted and executed.
* Lexical Environment:
  * Is a specification type.
  * Like `LexicalEnvironment` and `VariableEnvironment`.
  * Consists of:
    * `Environment Record`: identifier bindings.
    * `outer` reference to the outer lexical environment or `null`.

### Context
  * Is the value of `this` which is a reference to the object that owns the current executing code.
  * Determined by how a function is invoked.

### Scope
  * The region where the binding between a name (like variable name) to an entity is valid.

### Closure
  * A closure is a function that remembers its outer variables and can access them.
  * Combination of a function and the lexical environment within which that function was declared
  * The `closure` is the function object itself.
  * Accessing variables outside of the immediate lexical scope creates a closure.
  * Happens when we have a nested functions.
  * JavaScript engines also may optimize, discard variables that are unused to save memory.
  * A `Lexical Environment` object lives in the `heap` as long as there is a function which may use it. And when there are none, it is cleared.
  * All functions in JavaScript are closures.
  * The internal property `[[Environment]]` of a function, refers to the outer lexical environment

### IIFE
  * Immediately-invoked function expressions.
  * A design pattern used by most popular libraries to place all library code inside of a local scope.
  * No global property is created for the function (anonymous function expression).
  * All of the properties created inside of the function expression are scoped locally.
  * Encapsulation, preserve the global namespace as any variables declared within the function body will be local to the closure but will still live throughout runtime.
  * Benefits:
    * Local scoping.

### Hoisting
  * Variable and function **declarations** are put into memory during the compile phase.
  * Stays exactly where you typed it in your coding (not actually moved to the top).
  * Only hoists declarations, not initializations.
  * Declarations contribute to the `VariableEnvironment` when the execution scope is entered (^).

### Callback
  * A callback function is a function passed into another function as an argument.
  * To be invoked inside the called function at some point, like after an **asynchronous** operation has completed.

### Callback Hell
  * Deeply nested callbacks.
  * An example of `Pyramid of Doom`.

### Promise
  * Represents a value that either avaliable or will be avaliable in later.
  * Represents the completion or failure of an a deferred or asynchronous operation, and its resulting value.
  * The function passed with the arguments `resolve` and `reject` called the `executor` function. 
  * Can either be:
    * Fulfilled with a **value**.
    * Rejected with a **reason** (error).
  * Internal properties of promise instances:
    * `[[PromiseResult]]` the value or the error.
    * `[[PromiseState]]` can be: `pending`, `fulfilled` or `rejected`.
    * `[[PromiseFulfillReactions]]` queue for `then` handlers.
    * `[[PromiseRejectReactions]]` queue for `catch` handlers.
  * Methods:
    * `Promise.all(iterable)`
    * `Promise.race(iterable)` 
    * `Promise.resolve(value)` returns a resolved promise with the given value.
    * `Promise.reject(reason)` returns a rejected promise with the error.

### Pollyfills
  * Scripts that **fill in** the gap and add missing implementations for JavaScript or modifing the existing onces to support the modern standard.
  * Use the same API.
  * Like pollyfill for the `Function.prototype.bind` which is not supported in `IE8` or for promises.
  * Or pollyfills for new methods in ES6 to be used in all browsers.
  * Examples:
    * Babel polyfill.
    * polyfill.io.

### Fallback
  * Replaces the feature with:
    * Simplified functionality
    * Or Third-party plugin
    * Or an error message
  * Like if the browser doesn't support the video tag, replace it with flash plugin.

### Notes

* Every function invocation has both a scope and a context associated with it.
* When an execution context is created its `LexicalEnvironment` and `VariableEnvironment` components initially have the same value.
* The value of the `VariableEnvironment` component never changes.
* The value of the `LexicalEnvironment` component may change during execution of code within an execution context like when entering a block.
* Every run for a loop block has a separate `Lexical Environment`.

Promises example:

```js
function task1() {

  return new Promise(function(resolve, reject) {
    setTimeout(function() {
      console.log('Task1 is completed');
      resolve();
    }, 1100);
  });
  
}

function task2() {

  return new Promise(function(resolve, reject) {
    setTimeout(function() {
      console.log('Task2 is completed');
      resolve();
    }, 1200);
  });
  
}

function task3() {

  return new Promise(function(resolve, reject) {
    setTimeout(function() {
      console.log('Task3 is completed');
      resolve();
    }, 1300);
  });
  
}

task1()
  .then(task2)
  .then(task3);
```

Bind pollyfill:

```js
if (!('bind' in Function.prototype)) {
  Function.prototype.bind = function() {
    var func = this;
    var obj = arguments[0];
    var params = Array.prototype.slice.call(arguments, 1);
    return function() {
      func.apply(obj, params.concat(Array.prototype.slice.call(arguments)));
    }
  }
}
```

# Functions Tricks

### Decorator

Is a wrapper around a function that alters its behavior. Like caching the results or formatting them.

To implement them we need:

* `func.call(context, arg1, arg2…)` calls `func` with given context and arguments.
* `func.apply(context, args)` calls `func` with given context and arguments passed as an array-like.
* `func.bind(context, ...args)` returns a `bound variant` of function `func` that fixes the context and first arguments if given.

Note:

* `bind` is really useful with:
  * Setting the context for `setTimeout`'s callback function.
  * Partial functions.
  * Currying functions.

### Partial functions

Creates a new function by fixing some parameters of the existing one.

### Currying functions

* Translating a function from callable as `f(a, b, c)` into callable as `f(a)(b)(c)`.
* To allow it to be **called normally** or **get partials**.

### Call forwarding

A wrapper function that passes everything it gets to another one.

```js
function wrapper() {
  return printArgs.apply(this, arguments);
}
```
 
### Method borrowing

Using a method from another object on our object.

```js
Array.prototype.slice.call(arguments);
```

# Object

* Objects are associative arrays, stores key-value pairs.
* Keys must be `string` or `symbol`, values can be anything.
* To access a property, we can use:
  * The dot notation: `obj.key`
  * The square brackets notation `obj[key]`
* An empty object can be created using:
  * Using the object constructor `new Object()`.
  * Using the object literal `{}`.
* To remove a property we can use the `delete` operator.
* Existence check
  * Using `typeof`, `typeof obj.key !== 'undefined'`.
  * Using `key in obj`, better because if we have properties that store `undefined`.
* The equality `==` and strict equality `===` operators for objects work exactly the same.
* Cloning and merging
  * Shallow copy and merge
    * `Object.assign(dest[, src1, src2, src3...]);`
    * `Object.assign({}, user);`
    * `Object.assign` not supported in `IE`, we can use `for..in`.
    * `var clone = Object.create(Object.getPrototypeOf(obj), Object.getOwnPropertyDescriptors(obj));`
  * Deep copy
    * `_.cloneDeep()` from [Lodash](https://lodash.com/)
* There is other types of object other than the the `Object` (aka. `plain object`):
  * Array
  * Date
  * Error
* To check if object literal:
  `Object.prototype.toString.call(obj) === '[object Object]'`
* Global object in browsers is `window`.
* Global object in Node.js is `global`.

# OOP

### Constructor functions

* Are regular functions.
* As convention it should be named with capital letter first, and not called without `new`.
* When the function is executed as `new User()`, it does:
  * Create an empty object.
  * Assign the `[[prototype]]` of the created object to `User.prototype`.
  * Execute the function and assign `this` to the created object.
  * Return the value of `this` or the value of the `return` statement if the returned is an object.
* The default `prototype` is an object with the only property `constructor` that points back to the function itself.
* If we replace the default prototype as a whole, then there will be no `constructor` in it, unless we define manually.
* `myObj.constructor` is helpful, when we want to create another object from the same constructor that we don't know. `var myObj2 = new myObj.constructor()`.
* Any function can be run with `new`.
* To check if the function is called with `new` or without we can use `new.target` inside it.

To allow creating an object with `new` or without (not recommended):

```js
function User(name) {

  // Not called with new
  if (!new.target) {
    return new User(name);
  }

  this.name = name;
  
}
```

The `new function() { … }` pattern to encapsulate the creation of a single object:

```js
var user = new function() {
  this.name = 'Anas';
  this.age = 'Ali';
}
```
### Prototypes

* All objects have a hidden `[[Prototype]]` property that’s either another object or `null`.
* That prototype object has a prototype of its own, and so on until an object is reached with `null` as its prototype.
* Can be only one `[[Prototype]]` no multiple inheritance.
* The prototype is only used for **reading** properties.
* Write/delete operations work directly with the object.
* `delete obj.property` will have no effect if the property is inherited (prototype chain).
* `obj.property = 'value'` will create a property on this object regardless if there is property called `property` that inherited.
* `this` is not affected by prototypes at all.
* No matter where the method is found: in an object or its prototype. In a method call, `this` is always the object before the dot.
* `null` has no prototype, and acts as the final link in this **prototype chain**.
* Nearly all objects in JavaScript are instances of `Object`.
* To access the `[[Prototype]]`:
  * Use the non-standard property `__proto__`.
  * Use ES2015 `Object.getPrototypeOf()`, `Object.setPrototypeOf()`.
* `__proto__` is setter/getter for the `[[Prototype]]`, defined in `Object.prototype`.
* If an object that its prototype chain not end with the `Object`, the `__proto__` will not be avaliable.
* The `object instanceof contructor` operator examines the prototype chain for the check.
  * `var myObj = {}; console.log(myObj instanceof Object)` is `true` because `myObj.__proto__ == Object.prototype`

### Object.create()

Creates an empty object with given proto as `[[Prototype]]` (can be null) and optional property descriptors.

```js
Object.create(proto[, descriptors])
```

We can use `Object.create` to perform an object cloning:

```js
var clone = Object.create(Object.getPrototypeOf(obj), Object.getOwnPropertyDescriptors(obj));
```

To create an empty object without a prototype

```js
Object.create(null)
```

### Native prototypes

* JavaScript is prototype-based.
* There is no additional `[[Prototype]]` in the chain above `Object.prototype`.
* `Object.prototype.__proto__` is `null`.
* `Array.prototype.__proto__ == Object.prototype`.
* `arr.__proto__.__proto__ == Object.prototype`.
* Modifying native prototypes is not recommended unless for polyfills. 

### Class patterns

* Functional class pattern.
  * Create an object using `new` and function constructor that has everything inside it.
  * Public properties are defined inside the constructor function.
  * Public methods are defined inside the constructor function.
  * Can have `private` methods/properties that are not accessed from the outside.
* Factory class pattern.
  * Create an object by calling a function that returns a new object.
  * Public properties are defined inside the returned object.
  * Public methods are defined inside the returned object.
  * Can have `private` methods that are not accessed from the outside.
* Prototype-based class pattern.
  * Create an object using `new` and function constructor.
  * Public properties are defined inside the constructor function.
  * Public methods are defined inside the constructor's prototype property.
  * Can't have `private` methods, prefixing with `_` is a convention to indicate that a member should not be used from the outside.
  * The constructor only initializes the current object state.
  * More memory-efficient because all methods are shared between all objects.
  * Is the best and mostly used.

Note:

* For the `Functional class pattern` the `private` methods are simply defined in the scope of the constructor function, so any public member method will have an access to
outer scope so we can call them from any member function, but they will be executed in different context (doesn't have an access to the members of the current object `this`), to solve that we have more than one option:

  * When creating the private methods, bound them to the current context using `.bind(this)`.
  * Call the private methods using `.call` or `.apply` with `this` as the context.
  * Define a reference variable `var self = this` to the current object inside the constructor function and use it inside the private methods instead of `this`.

* For the `Factory class pattern` we can define a reference to the returned object inside the factory function and use it instead of `this` with above options.

Functional pattern example:

```js
function User(firstName, lastName) {

  var self = this;

  this.firstName = firstName;
  this.lastName = lastName;

  function getFirstName() {
    return self.firstName;
  }

  function getLastName() {
    return self.lastName;
  }

  this.getFullName = function() {
    return getFirstName() + ' ' + getLastName();
  }

}

var user = new User('Ahmad', 'Fares');

console.log(user.getFullName());
```

Factorial pattern example:

```js
function createUser(firstName, lastName) {

  var self = {};

  self.firstName = firstName;
  self.lastName = lastName;

  function getFirstName() {
    return self.firstName;
  }

  function getLastName() {
    return self.lastName;
  }

  self.getFullName = function() {
    return getFirstName() + ' ' + getLastName();
  }

  return self;

}

var user = createUser('Ahmad', 'Fares')

console.log(user.getFullName());
```

Prototype-based pattern example:

```js
function User(firstName, lastName) {
  this.firstName = firstName;
  this.lastName = lastName;
}

User.prototype._getFirstName = function() {
  return this.firstName;
}

User.prototype._getLastName = function() {
  return this.lastName;
}

User.prototype.getFullName = function() {
  return this._getFirstName() + ' ' + this._getLastName();
}

var user = new User('Ahmad', 'Fares');

console.log(user.getFullName());
```

### Inheritance

* Classical model
  * Classes are represented by constructor functions.
  * To inherit methods `Rabbit.prototype = Object.create(Animal.prototype);`.
  * Reset the constructor property `Rabbit.prototype.constructor = Rabbit;`.
  * To inherit properties `Animal.call(this, name);` inside the `Rabbit` constructor function.
* Prototypal model
  * Classes are just object litterals.
  * To inherit `var rabbit = Objct.create(animal)`.

# Functions

4 Ways to create a function:

* Function declaration:
  * Hoisted, Processed before the code block is executed. They are visible everywhere in the block.
  * Declarations contribute to the `VariableEnvironment` when the execution scope is entered.
* Function expression:
  * Created when the execution flow reaches them.
  * If they have a name assigned to them it is only visible by itself `var myFunc = function func() {...}`
* Function constructor
  `new Function('a', 'b', 'return a + b');`
* Arrow function.
  * Do not have `this`.
  * Do not have `arguments`.
  * Can’t be called with `new`.
  * Doesn't have their own `context`, works in the current one.

Notes:

* Properties of a function:
  * name: the name of the function
  * length: number of arguments
  * prototype
  * prototype.constructor
* \_\_proto\_\_: Function.prototype

# Scheduling with Timers

* Using `setTimeout` and `setInterval` functions.
* `setTimeout` can be used to implement `setInterval`.
* They are not a part of JavaScript specification (ES).
* Zero-timeout scheduling `setTimeout(...,0)`:
  * Is used to schedule the call as soon as possible, but after the current code is complete.
  * To split CPU-hungry tasks into pieces, so that the script doesn’t **hang**.
  * Let the browser do something else while the process is going on.

# Data Types

* Dynamically typed.
* There are data types, but variables are not bound to any of them.
* No `function` type, they belong to the object type, but `typeof` treats them differently.
* An old bug in JavaScript is `typeof null == 'object'`.
* Seven basic data type:
  * number
    * Serves both for integer and floating point numbers.
    * There are **special numeric values**: `Infinity`, `-Infinity` and `NaN`.
    * Maths is safe in JavaScript, will never stop with a fatal error.
    * `NaN` represents a computational error.
    * `1/0 == Infinity`;
    * `-1/0 == -Infinity`;
    * `"hello" / 2` evaluated to `NaN`.
  * string
    * Encoded using UTF-16.
    * Immutable, impossible to change a character.
  * boolean
  * null
    * A standalone type that has a single value `null`.
    * To indicate `empty` or `unknown values`.
  * undefined
    * A standalone type that has a single value `undefined`.
    * To indicate `value not assigned`
    * Only used for checks.
  * object
    * Not a primitive type.
    * Collection of data.
  * symbol
    * For unique identifiers.

```js
typeof undefined // "undefined"
typeof 0 // "number"
typeof true // "boolean"
typeof "foo" // "string"
typeof Symbol("id") // "symbol"
typeof Math // "object"
typeof null // "object"
typeof alert // "function"
```

# Type Conversion

### Implicit conversion

* `alert` automatically converts any value to a string.
* Mathematical operations convert values to numbers.
* If condition expression converted to boolean.

### Explicit conversion

#### To String

* `String(null)` become `'null'`
* `String(undefined)` become `'undefined'`
* `String(false)` become `'false'`
* `String(true)` become `'false'`
* `String(10.1)` become `'10.1'`

#### To Number

* `Number(null)` become `0`
* `Number(undefined)` become `NaN`
* `Number(false)` become `0`
* `Number(true)` become `1`
* `Number('10.1')` become `10.1`
* `Number('  10.1 \t\n ')` become `10.1`
* `Number('  10.1z \t\n ')` become `NaN`
* `Number('  \t\n ')` become `0`
* `Number('')` become `0`

Notes:

* To convert a string into a number
  * Trim whitespaces (spaces, tabs, new lines).
  * An empty string becomes `0`
  * An error gives `NaN`.
* Concatenation operation vs plus
  * `1 + 2 + 'a' == '3a'`
  * `'a' + 1 + 2 == 'a12'`
* Can convert to numbers with `+` and `-` also.
  * `+'10' == 10`
  * `-'10' == -10`

#### To Boolean

**False**: values that are intuitively **empty**

* `Boolean(null)` become `false`
* `Boolean(undefined)` become `false`
* `Boolean('')` become `false`
* `Boolean(0)` become `false`
* `Boolean(NaN)` become `false`

**True**: otherwise

* `Boolean(' ')` become `true`
* `Boolean('0')` become `true`
* `Boolean('abc')` become `true`
* `Boolean(123)` become `true`

Notes:

* happens in logical operations
* Methods of primitives:
  * Primitives except `null` and `undefined` provide many helpful methods.
  * These methods work via temporary wrapper objects, but JavaScript engines are well tuned to optimize that internally.
  * Like `'hello'.toUpperCase()`, `13.123.toFixed(2)`
* `new Number(10)` returns an `object` not primitive.

# Object to Primitive Conversion

* All objects will become `true` if converted to boolean.
* To convert to string, the `toString()` methods will be used or `valueOf().`
* To convert to number, the `valueOf()` methods will be used or `toString().`

# Operators

* The `,` comma operator.
  * `10,20 == 10`
  * Evaluates both, returns the last one.

# Comparisons

* Comparison operators return a logical value.
* Strings are compared letter-by-letter in the “dictionary” order.
* When values of different types are compared, they get converted to numbers (with the exclusion of a strict equality check).
* Values `null` and `undefined` equal `==` each other and do not equal any other value.
  * `null == null` is true.
  * `null == undefined` is true.
  * `undefined == undefined` is true.
* An incomparable undefined, same as `NaN` (always false):
  * `undefined > 0` is false.
  * `undefined == 0` is false.
  * `undefined < 0` is false.
  * `undefined <= 0` is false.

# Logical Operators

* Or (`||`)
  * OR returns the **first** truthy value or the **last** one if no such value is found.
* And (`&&`)
  * AND returns the **first** falsy value or the **last** value if none were found.
* The precedence of the `&&` is higher than `||`.

```js
(x > 0) && alert( 'Greater than zero!' );
```

```js
null || 2 || undefined // 2
```

```js
alert(1) || 2 || alert(3) // 2, since alert evaluated to undefined
```

```js
1 && null && 2 // null
```

### Arrow function examples

Multiple arguments

```js
var sum = (a, b) => a + b;
```

Single argument

```js
var double = n => n * 2;
```

Multiline arrow functions

```js
var double = (n) => {
  return n * 2;
};
```

### Default values

* If a parameter is not provided, then its value becomes undefined.
* Default value for the parameter (not supported in IE) `function func(name = 'Anas')`.
* Checking for `undefined` by `typeof name === 'undefined'`.
* By using `||` operator, `name = name || 'Anas'`.

Notes:

* A variable declared inside a function is only visible inside that function.
* The function is an object that its prototype points to `Function.prototype`

```js
hello.__proto__ == Function.prototype
```

# Use Strict

* Applies to entire scripts or to individual functions.
* Strict mode changes semantics. Relying on those changes will cause mistakes and errors in browsers which don't implement strict mode.
* `'use strict'` or `"use strict"` can be located at the top of a script or at the start of a function.
* Only comments may appear above `"use strict"`.
* There’s no way to cancel `use strict`.
* It is recommended.

### Some differences:

Makes it impossible to accidentally create global variables.

```js
mistypeVariable = 17;
```  

Throws an error when trying to delete undeletable property.

```js
delete Object.prototype;
```

Throws an error when trying to change unwritable property.

```js
var person = {};

Object.defineProperty(person, 'age', {
  value: 10,
  writable: false
});

person.age = 13; // TypeError
```

Throws an error when the a function arguments names are not unique.

```js
function func(a, a) {
  console.log(a);
}
```

No longer possible to reference the `global` object through `this` inside a function.

```js
function func() {
  console.log(this); // undefined
}

func();
```

The `caller`, `callee`, and `arguments` properties may not be accessed on strict mode functions.

```js
function func() {
  console.log(arguments.callee); // TypeError
}

func();
```
