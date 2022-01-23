# JS Basics
- If you use the + operator on strings that contain numbers, it concatenates them. If use any other mathematical operator (like *), it does the mathematical operation on the string numbers. 
    - Note: Concatenation takes place even if only one of the operands is a string.

## [JS Object Prototypes](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Object_prototypes)
- When you try to access a property/method in an object using dot notation (eg. `myObj.something`), the browser looks for `something` in the object. If it cant find it there it looks in the prototype of the object (stored in the `__proto__` property).
    - It does this recursively, working on through the prototype chain for the object, until the property is found.
    - It stops looking immediately after the first instance of `something` is found. This means that you can potentially have multiple objects in the prototype chain with the same property; only the closest one will be called.
- The root parent object of all objects is an object called `Object.prototype`. This object has its `__proto__` property set to null.
    - You can access the prototype of any object using `Object.getPrototypeOf(myObj)`.
- `const newObj = Object.create(otherObj)` method creates a new object and allows you to specify another object that will be used as the new object's prototype. The properties and methods of `otherObj` are therefore inherited by `newObj` and present in its prototype. 
- All _functions_ in JavaScript have a property called `prototype` (instead of the `__proto__` for Objects). 
    - When you call a constructor function, the value of this `prototype` is the value of `__proto__` in the newly created object.
    - You can  change the value prototype of a constructor function: `MyConstructorFunction.prototype = otherObj;` (otherwise it would just be the default object prototype).
