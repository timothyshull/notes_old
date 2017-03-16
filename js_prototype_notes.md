var obj = {};

var obj = Object.create(Object.prototype);

var obj = new Object();

all result in an empty object {} with one property - __proto__

properties on instantiated object __proto__:

__defineGetter__ - binds an object's property to the function used to look up that property
__defineSetter__ - binds an object's property to the function used to set/change that property
__lookupGetter__ - returns the function that is bound as a getter for the specified object property
__lookupSetter__ - returns the function that is bound as a setter for the specified object property
constructor - reference to the object function that instantiates the object by establishing the prototype chain fo the object and creating the properties on the object
hasOwnProperty - method that returns boolean specifying if the object has the specified property
isPrototypeOf - returns boolean specifying if an object exists in another object's prototype chain
propertyIsEnumerable - specifies whether a property can be enumerated by a for...in loop
toLocaleString - returns string representing elements of array
toString - returns string representing the specified object and its elements
valueOf - returns the primitive value of the object relative to booleans

get __proto__ - function to get the object's __proto__
set __proto__ - function to set the object's __proto__


properties on a function __proto__:

apply - a method that can be used to specify the this object and the arguments that the function is called with by passing an array-like object to it with this as the first arg and args array as second 
arguments (get arguments, set arguments) - an object (similar to array but not an array) representing the arguments that the function was called with
bind - creates a new function that when called has the this object set to the value passed to bind
call - call a function with the this object specified as the first argument and the reamining arguments as the arguments passed
caller (get caller, set caller) - specifies the function that called the currently executing function
constructor
length - number of arguments passed to the function
name - specifies the name of a function
toString
__proto__
(also contains an internal reference to it's scope)


properties on an empty anonymous function - created with Function()/new Function()

arguments 
caller
length
name
prototype
__proto__


properties on the Object function:

arguments
caller
create - (proto[, propertiesObject]) creates a new object with the specified prototype object and the additional properties
defineProperties - adds properties to an object
defineProperty - adds one property to an object
deliverChangeRecords - ES6 feature related to Object.observe. Synchronously flushes any change records stored in an Object.observe callback's queue
freeze - prevents new properties from being added to a frozen object
getNotifier - changes made to properties using get and set are not available to observe. getNotifier is used to define custom get and set methods, and provides a notify method that allows one to define the object that observe is called with. 
getOwnPropertyDescriptor
getOwnPropertyNames
getOwnPropertySymbols
getPrototypeOf
is - determines whether two values are the same (similar to assert)
isExtensible
isFrozen
isSealed
keys - returns an array of all of the object's enumerable properties
length
name
observe - a method of Object that allows a callback to be specified that will be called each time an object is changed (takes a change object that has properties name, type, and object) - this is an ES6 means of data binding
preventExtensions - stops the object from being extended (ie new properties being added)
prototype
seal - stops new properties from being added and marks all existing properties as non-configurable. Existing properties can still be modified as long as they are writable.
setPrototypeOf
unobserve
__proto__
<function scope>

prototype is a property of a function object - it shows the prototype of objects constructed by that function.

__proto__ is an internal property of an object that points to the object's prototype (internal [[Prototype]])

The safe way to access this is Object.getPrototypeOf(Object);



defineProperty opens access to internal property attributes. Porperties are either data descriptors - a property which has a value, or accessor descriptors - a property which is described by a getter-setter pair of functions. 

The internal attributes include:

For both data and accessor descriptors:
configurable - true if and only if the type of this property descriptor may be changed and if the property may be deleted from the corresponding object. Defaults to false.
enumerable - true if and only if this property shows up during enumeration of the properties on the corresponding object. Defaults to false.

For just data descriptors:
value - The value associated with the property. Can be any valid JavaScript value (number, object, function, etc). Defaults to undefined.
writable - true if and only if the value associated with the property may be changed with an assignment operator. Defaults to false.
get - A function which serves as a getter for the property, or undefined if there is no getter. The function return will be used as the value of property. Defaults to undefined.
set - A function which serves as a setter for the property, or undefined if there is no setter. The function will receive as only argument the new value being assigned to the property. Defaults to undefined.