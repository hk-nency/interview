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







