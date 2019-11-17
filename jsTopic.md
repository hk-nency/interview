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







































