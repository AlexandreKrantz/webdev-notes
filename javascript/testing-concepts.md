## Testing Concepts
- **Test Driven Development (TDD):** write the tests before you write the code.
	- It's easier to write tests before you write the code. You know what the code is supposed to do in theory, so you can just do some examples of arguments and their expected return values.
	- **Triangulation:** write tests that outline all the functionalities of the function you want to write. 
- **Unit testing:** divide you software project into the smallest possible units and test each unit individually as you code them.
	- Contrast unit tests with **integration tests**, which test integrations between two or more units, and functional tests, which test complete user interaction workflows.

### Reducing Coupling
- Units of code that are highly inter-dependent are **tightly coupled**. You want to try to reduce coupling as much as possible so that each unit can be tested in isolation (without depending on other functions behaving correctly). TDD encourages this.
	- **Isolate side-effects** from the rest of your program logic. That means don’t mix logic with I/O (including network I/O, rendering UI, logging, etc…).

#### Mocking
- **[Mocking](https://medium.com/javascript-scene/mocking-is-a-code-smell-944a70c90a6a)** is a technique to test tightly coupled code by making placeholder functions that always return the value you want. 
	- That way you don't need to deal with variable outside actors like the DOM, requests, etc. Also, it makes your tests run faster.
	- If you find that it’s hard to write unit tests for your program without mocking lots of other things, that’s a sign that your program is not modular enough.
- **You shouldn't aim for 100% code coverage** with your tests, or you will find yourself mocking everything and massively complicating your codebase. Especially if parts of your code no real logic, you shouldn't need to do any testing.
	- Instead of code coverage, you want to maximize **case coverage**

#### Pure Functions
- **You should aim for [pure functions]([JavaScript: What Are Pure Functions And Why Use Them? | by James Jeffery | Medium](https://medium.com/@jamesjefferyuk/javascript-what-are-pure-functions-4d4d5392d49c))** that do one thing. This is not a 100% achievable goal in practice, but it will help reduce the amount of mocking. 
	- If you’re passed an array or an object, and you want to return a changed version of that object, you should create a new copy of the object with the required changes. You can enforce immutability with a library like [ImmutableJS](https://immutable-js.com/)

#### Declarative Composition
- **Function composition** is the process of applying a function to the return value of another function. You are essentially piping multiple functions together. It is better practice to do a declarative composition (where each module is unit tested), than an imperative composition (which has some untested logic). 
``` javascript
// Function composition OR  
// import pipe from 'lodash/fp/flow';  
const pipe = (...fns) => x => fns.reduce((y, f) => f(y), x);// Functions to compose  
const g = n => n + 1;  
const f = n => n * 2;

// Imperative composition  
const doStuffBadly = x => {  
  const afterG = g(x);  
  const afterF = f(afterG);  
  return afterF;  
};

// Declarative composition  
const doStuffBetter = pipe(g, f);
```

#### Pub/Sub Pattern
- In the publish/subscribe pattern, units don’t directly call each other. Instead, they publish messages that other units (subscribers) can listen to. Publishers don’t know what (if any) units will subscribe, and subscribers don’t know what (if any) publishers will publish.
- Todo: [Learn more here](https://medium.com/@thebabscraig/javascript-design-patterns-part-2-the-publisher-subscriber-pattern-8fe07e157213)

### What to Test
- **Origins for messages**: Incoming, outgoing, sent to self. 
- **Flavors for messages**: query (return something, change nothing), command (return nothing, change something). 
	- Sometimes a message is both a command and a query.

- Test *incoming query messages* by making assertions about what they send back.
	- Test the interface, not the implementation. The object/function remains a black box.
- Test *incoming command messages* by making assertions about direct public side effects. 
- In general, don't bother testing messages sent to self or private methods.

![[Pasted image 20220804122042.png]]

### Testing library: [[jest |Jest]]