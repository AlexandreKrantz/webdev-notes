## Testing Basics
- **Test Driven Development (TDD):** write the tests before you write the code.
	- It's easier to write tests before you write the code. You know what the code is supposed to do in theory, so you can just do some examples of arguments and their expected return values.
	- **Triangulation:** write tests that outline all the functionalities of the function you want to write. 
- **Unit testing:** divide you software project into the smallest possible units and test each unit individually as you code them.
	- Reduces the complexity of testing/debugging.

## Jest
- **Jest** is a test-running library. Other ones have a similar syntax. Add it to your project with `npm i --save-dev jest`.
- Basic syntax:
```javascript
test('two plus two is four', () => {
  expect(myAddFunction(2,2)).toBe(4);
});
```  
- `toBe` is one type of "matcher" that tests for exact equality with the value that is passed into it. 
	- There are [other types of matchers](https://jestjs.io/docs/using-matchers) that can be used to test for ranges, regular expressions, or compare objects, etc.
- By default, the current version of Jest will not recognize ES6 import statements. You should use [[es6-babel#Babel |Babel]] to transpile to ES5 ([specific instructions](https://jestjs.io/docs/en/getting-started#using-babel))
- 