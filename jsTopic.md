1. functions are objects in JS?explain it.

In JavaScript, functions are objects. You can work with functions as if they were objects. For example, you can assign functions to variables, to array elements, and to other objects. They can also be passed around as arguments to other functions or be returned from those functions.

Let's first run a small test and confirm that a function is indeed an object instance:

function message() {
    alert("Greetings Linda!");
}
alert(typeof message);                   // => function
alert(message instanceof Object);        // => true


Functions are copied by reference

function original() {
    // ...
}
original.message = "Hello";
var copy = original;
alert(original.message);         // => Hello
alert(copy.message);             // => Hello
copy.message = "Greetings";
alert(original.message);         // => Greetings
alert(copy.message);             // => Greetings




///////////// JSONP

JSONP (JSON with Padding) is a method commonly used to bypass the cross-domain policies in web browsers. (You are not allowed to make AJAX requests to a web page perceived to be on a different server by the browser.)

JSON and JSONP behave differently on the client and the server. JSONP requests are not dispatched using the XMLHTTPRequest and the associated browser methods. Instead a <script> tag is created, whose source is set to the target URL. This script tag is then added to the DOM (normally inside the <head> element).


//////coercion

Type coercion is the process of converting value from one type to another (such as string to number, object to boolean, and so on). 

Implicit vs. explicit coercion
Type coercion can be explicit and implicit.

When a developer expresses the intention to convert between types by writing the appropriate code, like Number(value), it’s called explicit type coercion (or type casting).

Since JavaScript is a weakly-typed language, values can also be converted between different types automatically, and it is called implicit type coercion. It usually happens when you apply operators to values of different types, like
1 == null, 2/’5', null + new Date(), or it can be triggered by the surrounding context, like with if (value) {…}, where value is coerced to boolean.



//////////Event Bubble VS Capture

Event bubbling and capturing are two ways of event propagation in the HTML DOM API, when an event occurs in an element inside another element, and both elements have registered a handle for that event.


With bubbling, the event is first captured and handled by the innermost element and then propagated to outer elements.

With capturing, the event is first captured by the outermost element and propagated to the inner elements.


target.addEventListener(type, listener[, useCapture]);

Capturing - true
Bubbling - false

Default - Bubble


document.querySelector('.box-1').addEventListener('click', e => {
    console.log('Box-1 is clicked');
});


Event Order

first capturing and then bubbling


exmaple - https://stackoverflow.com/questions/4616694/what-is-event-bubbling-and-capturing

------StopPropagation

 1. event.stopPropagation() :  Whenever a event is raised, event will propagate or bubble up till the window object level.

Ex: Parent Div containing a Child Div and both are registered for click events. When Child Div clicked , event handlers for both Child Div and Parent Div will be fired.

Click here for demo

To avoid the event bubbling  to top level DOM hierarchy , use the event.stopPropagation().

Click here for demo

-------StopImmediatePropagation

2. event.stopImmediatePropagation(): The event handlers will be called in the order they have registered. Lets say for a Div element click event is registered from different places. Then when the  Div element is clicked, the click event handler will be fired in all the places.

Since event.stopPropagation() will only stop event propagation to parent level and  not at the same element level, so to avoid the event firing at multiple places  we have to use event.stopImmediatePropagation().


-----event.preventDefault()


Prevents the browsers default behaviour (such as opening a link), but does not stop the event from bubbling up the DOM.


Closures---

Closures is when functi remeber and access its lexical scope even when it is called outside its lexical scope.

Usage--

1.
Among other things, closures are commonly used to give objects data privacy. Data privacy is an essential property that helps us program to an interface, not an implementation. This is an important concept that helps us build more robust software because implementation details are more likely to change in breaking ways than interface contracts.

In JavaScript, closures are the primary mechanism used to enable data privacy. When you use closures for data privacy, the enclosed variables are only in scope within the containing (outer) function. You can’t get at the data from an outside scope except through the object’s privileged methods. In JavaScript, any exposed method defined within the closure scope is privileged.

a = (function () {
    var privatefunction = function () {
        alert('hello');
    }

    return {
        publicfunction : function () {
            privatefunction();
        }
    }
})();

As you can see there, a is now an object, with a method publicfunction ( a.publicfunction() ) which calls privatefunction, which only exists inside the closure. You can NOT call privatefunction directly (i.e. a.privatefunction() ), just publicfunction().

2. used as function curring

Now you want a function that adds 10 to any number. We’ll call it `add10()`. The result of `add10(5)` should be `15`. Our `partialApply()` function can make that happen.


------------------Arrow function vs normal functions-----------------------


1. Lexical this and arguments

Arrow functions don't have their own this or arguments binding. Instead, those identifiers are resolved in the lexical scope like any other variable. That means that inside an arrow function, this and arguments refer to the values of this and arguments in the environment the arrow function is defined in (i.e. "outside" the arrow function):



// Example using a function expression
function createObject() {
  console.log('Inside `createObject`:', this.foo);
  return {
    foo: 42,
    bar: function() {
      console.log('Inside `bar`:', this.foo);
    },
  };
}

createObject.call({foo: 21}).bar(); // override `this` inside createObject

//Inside `createObject`: 21
Inside `bar`: 42


// Example using a arrow function
function createObject() {
  console.log('Inside `createObject`:', this.foo);
  return {
    foo: 42,
    bar: () => console.log('Inside `bar`:', this.foo),
  };
}

createObject.call({foo: 21}).bar(); // override `this` inside createObject

Inside `createObject`: 21
Inside `bar`: 21

In the function expression case, this refers to the object that was created inside the createObject. In the arrow function case, this refers to this of createObject itself.

This makes arrow functions useful if you need to access the this of the current environment:

// currently common pattern
var that = this;
getData(function(data) {
  that.data = data;
});

// better alternative with arrow functions
getData(data => {
  this.data = data;
});


2. Using new keyword
Regular functions created using function declarations or expressions are constructible and callable. Since regular functions are constructible, they can be called using the new keyword.
However, the arrow functions are only callable and not constructible, i.e arrow functions can never be used as constructor functions. Hence, they can never be invoked with the new keyword.


3. Arguments binding
Arrow functions do not have an arguments binding. However, they have access to the arguments object of the closest non-arrow parent function. Named and rest parameters are heavily relied upon to capture the arguments passed to arrow functions.


































