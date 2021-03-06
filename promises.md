**Promises**

Promises are used to handle asynchronous operations in JavaScript. They are easy to manage when dealing with multiple asynchronous operations where callbacks can create callback hell leading to unmanageable code.

Prior to promises events and callback functions were used but they had limited functionalities and created unmanageable code.
Multiple callback functions would create callback hell that leads to unmanageable code.
Events were not good at handling asynchronous operations.

Promises are the ideal choice for handling asynchronous operations in the simplest manner. They can handle multiple asynchronous operations easily and provide better error handling than callbacks and events.



Benefits of Promises

Improves Code Readability

Better handling of asynchronous operations

Better flow of control definition in asynchronous logic

Better Error Handling

A Promise has four states:

fulfilled: Action related to the promise succeeded

rejected: Action related to the promise failed

pending: Promise is still pending i.e not fulfilled or rejected yet

settled: Promise has fulfilled or rejected

A promise can be created using Promise constructor.

Syntax

var promise = new Promise(function(resolve, reject){
     //do something
});
Parameters

Promise constructor takes only one argument,a callback function.
Callback function takes two arguments, resolve and reject
Perform operations inside the callback function and if everything went well then call resolve.
If desired operations do not go well then call reject.


var promise = new Promise(function(resolve, reject) { 

const x = "geeksforgeeks"; 
const y = "geeksforgeeks"
if(x === y) { 
	resolve(); 
} else { 
	reject(); 
} 
}); 

promise. 
	then(function () { 
		console.log('Success, You are a GEEK'); 
	}). 
	catch(function () { 
		console.log('Some error has occured'); 
	}); 



Polyfill of promise
// class Promise {


//     constructor(executor) {
//         this.resolutionStack = [];
//         executor(this.resolver.bind(this))
//     }

//     resolver() {
//         while (this.resolutionStack.length > 0) {
//             let resolverhandler = this.resolutionStack.shift();
//             resolverhandler();
//         }

//     }
//     then(resolutionhandler) {
//         this.resolutionStack.push(resolutionhandler);
//     }

// }



class MyPromise {

    constructor(executor) {
        this._resolutionQueue = [];
        this._rejectionQueue = [];
        this._state = 'pending';
        this._value;
        this._rejectionReason;

        try {
            executor(this._resolve.bind(this), this._reject.bind(this));
        } catch (e) {
            this._reject(e);
        }
    }

    _runRejectionHandlers() {

        while (this._rejectionQueue.length > 0) {
            var rejection = this._rejectionQueue.shift();

            try {
                var returnValue = rejection.handler(this._rejectionReason);
            } catch (e) {
                rejection.promise._reject(e);
            }

            if (returnValue && returnValue instanceof MyPromise) {
                returnValue.then(function (v) {
                    rejection.promise._resolve(v);
                }).catch(function (e) {
                    rejection.promise._reject(e);
                });
            } else {
                rejection.promise._resolve(returnValue);
            }
        }
    }

    _runResolutionHandlers() {
        console.log("this._resolutionQueue.", this._resolutionQueue);
        while (this._resolutionQueue.length > 0) {
            var resolution = this._resolutionQueue.shift();

            try {
                var returnValue = resolution.handler(this._value);
            } catch (e) {
                resolution.promise._reject(e);
            }

            if (returnValue && returnValue instanceof MyPromise) {
                returnValue.then(function (v) {
                    resolution.promise._resolve(v);
                }).catch(function (e) {
                    resolution.promise._reject(e);
                });
            } else {
                resolution.promise._resolve(returnValue);
            }
        }
    }

    _reject(reason) {
        if (this._state === 'pending') {
            this._rejectionReason = reason;
            this._state = 'rejected';

            this._runRejectionHandlers();

            while (this._resolutionQueue.length > 0) {
                var resolution = this._resolutionQueue.shift();
                resolution.promise._reject(this._rejectionReason);
            }
        }
    }

    _resolve(value) {
        if (this._state === 'pending') {
            this._value = value;
            this._state = 'resolved';

            this._runResolutionHandlers();
        }
    }

    then(resolutionHandler, rejectionHandler) {


        console.log("this._resolutionQueue.", this._resolutionQueue);
        var newPromise = new MyPromise(function () { });

        this._resolutionQueue.push({
            handler: resolutionHandler,
            promise: newPromise
        });

        if (typeof rejectionHandler === 'function') {
            this._rejectionQueue.push({
                handler: rejectionHandler,
                promise: newPromise
            });
        }

        if (this._state === 'resolved') {
            this._runResolutionHandlers();
        }

        if (this._state === 'rejected') {
            newPromise._reject(this._rejectionReason);
        }

        return newPromise;
    }

    catch(rejectionHandler) {
        var newPromise = new MyPromise(function () { });

        this._rejectionQueue.push({
            handler: rejectionHandler,
            promise: newPromise
        });

        if (this._state === 'rejected') {
            this._runRejectionHandlers();
        }

        return newPromise;
    }

}


var promise = new MyPromise(function (resolve, reject) {
    setTimeout(function () {
        resolve("first resolve");
    }, 50000)
})

promise.then(function (value) {

    console.log("second callback starts");

    return new Promise(function (resolve, reject) {
        setTimeout(function () {
            resolve("second resolve");
        }, 500000)
    })
}).then(function () {
    console.log("second handler");
})


// promise.then(function () {
//     console.log("bye")
// })








#Genrator Functions

A generator is a function that can stop midway and then continue from where it stopped

function * generatorFunction() { // Line 1

  console.log('This will be executed first.');
  
  yield 'Hello, ';   // Line 2
  
  console.log('I will be printed after the pause'); 
  
  yield 'World!';
  
}

const generatorObject = generatorFunction(); // Line 3

console.log(generatorObject.next().value); // Line 4

console.log(generatorObject.next().value); // Line 5

console.log(generatorObject.next().value); // Line 6

// This will be executed first.
// Hello, 
// I will be printed after the pause
// World!
// undefined

In JavaScript, a generator is a function which returns an object on which you can call next(). Every invocation of next() will return an object of shape —
{ 
  value: Any,
  done: true|false
} 
The value property will contain the value. The done property is either true or false. When the done becomes true, the generator stops and won’t generate any more values.



#----------------_Async And Await

JavaScript async/await is a relatively new way to handle asynchronous operations in JavaScript. It gives you power new to make your code shorter and more understandable.

Before async/await, callbacks and promises were used to handle asynchronous calls in JavaScript. Everyone who learns JavaScript has heard about the so-called callback hell.  However, I won’t be writing about this because callbacks aren’t the main focus of this post.

function caserUpper(val) {
  return new Promise((resolve, reject) => {
    resolve(val.toUpperCase());
  });
}

async function msg(x) {
  try {
    const msg = await caserUpper(x);
    console.log(msg);
  } catch(err) {
    console.log('Ohh no:', err.message);
  }
}

msg('Hello'); // HELLO
msg(34); // Ohh no: val.toUpperCase is not a function




