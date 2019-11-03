#Objects

Different ways of creating objects in JS

1. Object literal

var human = {
	firstName: "Virat",
	lastName: "Kohli",
	age: 30,
	fullName: function(){
		return this.firstName + " " + this.lastName		
	}
}

2. Using new Object() syntax

var human = new Object()
console.log(human);// Creates an empty object

3.Object Constructor

Constructor function in JavaScript is used for creating new objects using a blueprint. Just like classes are used for creating objects in Java, C# we can use constructors to create objects in JavaScript

function Human(firstName, lastName) {
	this.firstName = firstName,
	this.lastName = lastName,
	this.fullName = function() {
		return this.firstName + " " + this.lastName;
	}
}

var person1 = new Human("Virat", "Kohli");

console.log(person1)

4. Object.create

Object.create methods accepts two arguments.

prototypeObject: Newly created objects prototype object. It has to be an object or null.
propertiesObject: Properties of the new object. This argument is optional
Create object with Object.create with no prototype

var object = Object.create(null);

Here, we have created a new object person using Object.create method. As we have passed null for the prototypeObject. person object does not have any prototype object.

var person ={
 fullname: function(){
   console.log("firstname"+ this.firstName + "secondName" + this.secondName);
 }
 
 var obj = Object.create(person);
 obj.firstName = "Hervinder";
 obj.secondName ="Singh"
 
 obj.fullName(): // Hervinder Singh
}


Object.create 2nd argument — propertiesObject

propertiesObject is used to create properties on new object. It acts as a descriptor for the new properties to be defined. Descriptors can be data descriptor or access descriptors.

Data descriptors are

configurable
enumerable
value
writable


///////////////////Object.create polyfill

function createAndLinkObject(o) {
	function F(){}
    F.prototype = o;
    var f = new F();
	return f;
}

var anotherObject = {
	a: 2
};

var myObject = createAndLinkObject( anotherObject );

myObject.a; // 2



*******************Prototypes in JavaScript*************

Consider the constructor function below:

function Human(firstName, lastName) {
	this.firstName = firstName,
	this.lastName = lastName,
	this.fullName = function() {
		return this.firstName + " " + this.lastName;
	}
}

var person1 = new Human("Virat", "Kohli");

console.log(person1)

Let’s create objects person1 and person2 using the Human constructor function:

var person1 = new Human("Virat", "Kohli");
var person2 = new Human("Sachin", "Tendulkar");

On executing the above code, the JavaScript engine will create two copies of the constructor function, each for person1 and person2.

Problem 
i.e. every object created using the constructor function will have its own copy of properties and methods. It doesn’t make sense to have two instances of function fullName that do the same thing. Storing separate instances of function for each object results into wastage of memory. We will see as we move forward, how we can solve this issue.


Prototype

When a function is created in JavaScript, the JavaScript engine adds a prototype property to the function. This prototype property is an object (called as prototype object) which has a constructor property by default. The constructor property points back to the function on which prototype object is a property. We can access the function’s prototype property using functionName.prototype.

To access the prototype property of the Human constructor function, use the below syntax:

console.log(Human.prototype);

prototype property of the function is an object (prototype object) with two properties:

1.constructor property which points to Human function itself
2. __proto__ property: We will discuss this while explaining inheritance in JavaScript

When an object is created in JavaScript, JavaScript engine adds a __proto__ property to the newly created object which is called dunder proto. dunder proto or __proto__ points to the prototype object of the constructor function.

var person2 = new Human("Sachin", "Tendulkar");
console.log(person2);

Human.prototype === person2.__proto__ //true
person1.__proto__ === person2.__proto__ //true



Prototype object of the constructor function is shared among all the objects created using the constructor function.

Prototype Object


As prototype object is an object, we can attach properties and methods to the prototype object. Thus, enabling all the objects created using the constructor function to share those properties and methods.

The new property can be added to the constructor function’s prototype property using either the dot notation or square bracket notation as shown below:



//Create an empty constructor function
function Person(){

}
//Add property name, age to the prototype property of the Person constructor function
Person.prototype.name = "Ashwin" ;
Person.prototype.age = 26;
Person.prototype.sayName = function(){
	console.log(this.name);
}

//Create an object using the Person constructor function
var person1 = new Person();

//Access the name property using the person object
console.log(person1.name)// Output" Ashwin


Let’s create another object person2 using the Person constructor function.

var person2 = new Person();
//Access the name property using the person2 object
console.log(person2.name)// Output: Ashwin

Now, let’s define a property name on the person1 object

person1.name = "Anil"
console.log(person1.name)//Output: Anil
console.log(person2.name)//Output: Ashwin


**********Problems with the prototype 

As prototype object is shared among all the objects created using the constructor function, its properties and methods are also shared among all the objects. If an object A modifies property of the prototype having primitive value, other objects will not get affected by, as object A will create a property on its objects



Combine Constructor/Prototype
To solve the problems with the prototype and the problems with the constructor, we can combine both the constructor and function.
Problem with the constructor function: Every object has its own instance of the function
Problem with the prototype: Modifying a property using one object reflects the other object also
To solve both problems, we can define all the object-specific properties inside the constructor and all shared properties and methods inside the prototype as shown below:

//Define the object specific properties inside the constructor

function Human(name, age){
	this.name = name,
	this.age = age,
	this.friends = ["Jadeja", "Vijay"]
}

//Define the shared properties and methods using the prototype
Human.prototype.sayName = function(){
	console.log(this.name);
}

//Create two objects using the Human constructor function
var person1 = new Human("Virat", 31);
var person2 = new Human("Sachin", 40);

//Lets check if person1 and person2 have points to the same instance of the sayName function
console.log(person1.sayName === person2.sayName) // true

//Let's modify friends property and check
person1.friends.push("Amit");

console.log(person1.friends)// Output: "Jadeja, Vijay, Amit"
console.log(person2.friends)//Output: "Jadeja, Vijay"


Here as we have wanted each object to have their own name, age, and friends property. Hence, we have defined these properties inside the constructor using this. However, as sayName is defined on the prototype object, it will be shared among all the objects.


For More Understanding https://medium.com/better-programming/prototypes-in-javascript-5bba2990e04b



**************Prototype Chaining

Prototype chaining means an object’s dunder proto or proto property will point to another object instead of pointing to the constructor function prototype. If the other object’s dunder proto or proto property points to another object it will result in the chain. This is called Prototype Chaining.


Problems with prototype chaining
As all the properties of the super type prototype are shared among the child objects, if one child modifies the property of the super type prototype, other children also get affected. This

function foo(name){
    this.name = name
}

foo.prototype.myName = function(){

    return "foo return" `${this.name}`; 
}

function bar(name,label){
       foo.call(this,name);
       this.label = label;
} 

bar.prototype = Object.create(foo.prototype);

bar.prototype.myLabel = function(){
    return "foo return" `${this.label}`; 
}

var a = new bar();
a.myName();
a.myLabel();


https://medium.com/javascript-scene/master-the-javascript-interview-what-s-the-difference-between-class-prototypal-inheritance-e4cd0a7562e9

