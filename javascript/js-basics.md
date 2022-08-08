# JS Basics
- If you use the + operator on strings that contain numbers, it concatenates them. If use any other mathematical operator (like *), it does the mathematical operation on the string numbers. 
    - Note: Concatenation takes place even if only one of the operands is a string.

- A number 0, an empty string "", null, undefined, and NaN all become false. Because of that they are called “falsy” values.
- Other values become true, so they are called “truthy”.

## Data Types
- JS is _dynamically typed_, this means that there exist data types, but variables are not bound to them. For example, a variable can at one moment be a string and then store a number.
- The number type represents both integer and floating point numbers. This also includes _special numeric values_:
    - `Infinity` is a value greater than any other number. 
    - `NaN`
- BigInt type is used for integers greater than max number length.
- Strings are surrounded by quotes. No difference between single and double quotes in JS.
- Boolean type is either true or false.
- `null` value is assigned.
- `undefined` value means "is not assigned" (you technically can assign it also, but that goes against its intended purpose).
- Objects are not a primitive type because they can store collections of data and more complex entities. They are also stored by reference. 
- Symbol type is used to create indentifiers for objects (uncommon).

## [JS Object Prototypes](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Object_prototypes)
Example of an constructor function (Constructors start with a capital letter):
``` JS
function Person(name) {
  this.name = name;
  this.introduceSelf = function() {
    console.log(`Hi! I'm ${this.name}.`);
  }
}
const frankie = new Person('Frankie');
```
- When you try to access a property/method in an object using dot notation (eg. `myObj.something`), the browser looks for `something` in the object. If it cant find it there it looks in the prototype of the object (stored in the `__proto__` property).
    - It does this recursively, working on through the prototype chain for the object, until the property is found.
    - It stops looking immediately after the first instance of `something` is found. This means that you can potentially have multiple objects in the prototype chain with the same property; only the closest one will be called.
    - Properties that are defined directly in the object (not in the prototype of the object), are called own properties. You can check whether a property is an own property using the static `Object.hasOwn(myObj, propertyName)` method.
- The root parent object of all objects is an object called `Object.prototype`. This object has its `__proto__` property set to null.
    - You can access the prototype of any object using `Object.getPrototypeOf(myObj)`.
- `const newObj = Object.create(otherObj)` method creates a new object and allows you to specify another object that will be used as the new object's prototype. The properties and methods of `otherObj` are therefore inherited by `newObj` and present in its prototype. 
- All _functions_ in JavaScript have a property called `prototype` (instead of the `__proto__` for Objects). 
    - When you call a constructor function, the value of this `prototype` is the value of `__proto__` in the newly created object.
    - You can  change the value prototype of a constructor function: `MyConstructorFunction.prototype = otherObj;` (otherwise it would just be the default object prototype).

## [Object-oriented programming](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Object-oriented_programming)
- In JavaScript, we can and often do create objects without any separate class definition, either using a function or an object literal. This can make working with objects much more lightweight than it is in classical OOP.
- Inheritance works with the prototype chain. each level of the hierarchy is represented by a separate object, and they are linked together via the __proto__ property. 
    - The prototype chain's behavior is less like inheritance and more like delegation. Delegation is a programming pattern where an object, when asked to perform a task, can perform the task itself or ask another object (its delegate) to perform the task on its behalf.

## Understanding Errors
- An error is a type of object built into the JS language, consisting of a name/type and a message. 
- A `ReferenceError` is thrown when one refers to a variable that is not declared and/or initialized within the current scope. 
- A `TypeError` is thrown for the following reasons:
    - an operand or argument passed to a function is incompatible with the type expected by that operator or function;
    - or when attempting to modify a value that cannot be changed;
    - or when attempting to use a value in an inappropriate way.

## Clean Code
- You should always write your code as if comments didn't exist. This forces you to write your code in the simplest, plainest, most self-documenting way you can humanly come up with. 
- Make sure variable and function names are self descriptive, and any complex functionality as wrapped up in a function.
- Only at the point where the code cannot be made easier to understand should you begin to add comments.

## DOM Manipulation and Events
