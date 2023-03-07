# JS Design Patterns
## Object Constructors
```javascript
const Person = function(name, age) {
  this.sayHello = () => console.log('hello!');
  this.name = name;
  this.age = age;
};

const jeff = new Person('jeff', 27);
```
One of the biggest issues with constructors is that while they _look_ just like regular functions, they do not behave like regular functions at all. If you try to use a constructor function without the `new` keyword, your program will not work as expected, but it won’t produce error messages that are easy to trace.

## Factory Functions
The factory function pattern is similar to constructors, but instead of using `new` to create an object, factory functions simply set up and return the new object when you call the function. Check out this example:
```javascript
const personFactory = (name, age) => {
  const sayHello = () => console.log('hello!');
  return { name, age, sayHello };
};

const jeff = personFactory('jeff', 27);

console.log(jeff.name); // 'jeff'

jeff.sayHello(); // calls the function and logs 'hello!'
```

[Another example]([Factory Functions and the Module Pattern | The Odin Project](https://www.theodinproject.com/lessons/node-path-javascript-factory-functions-and-the-module-pattern#back-to-factory-functions))

### Inheritance with Factory Functions
```javascript
const Person = (name) => {
  const sayName = () => console.log(`my name is ${name}`);
  return {sayName};
}

const Nerd = (name) => {
  // simply create a person and pull out the sayName function with destructuring assignment syntax!
  const {sayName} = Person(name);
  const doSomethingNerdy = () => console.log('nerd stuff');
  return {sayName, doSomethingNerdy};
}

const jeff = Nerd('jeff');

jeff.sayName(); //my name is jeff
jeff.doSomethingNerdy(); // nerd stuff
```

## Module Pattern
```javascript
const calculator = (() => {
  const add = (a, b) => a + b;
  const sub = (a, b) => a - b;
  const mul = (a, b) => a * b;
  const div = (a, b) => a / b;
  return {
    add,
    sub,
    mul,
    div,
  };
})();

calculator.add(3,5); // 8
calculator.sub(6,2); // 4
calculator.mul(14,5534); // 77476
```
- The concept is simple: write a function, wrap it in parentheses, and then immediately call the function by adding `()` to the end of it.
- It is essentially a factory function that can only be used up. It's a good way of bundling up a bunch of related methods and keeping them out of the global scope. 