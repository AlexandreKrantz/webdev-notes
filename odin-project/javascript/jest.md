## Jest
- Jest was created by Facebook and integrates well with many JavaScript libraries and frameworks like React, Angular and Vue to name a few.
- **Jest** is a test-running library. Other ones have a similar syntax. Add it to your project with `npm i --save-dev jest`.
- Basic syntax:
```javascript
test('two plus two is four', () => {
  expect(myAddFunction(2,2)).toBe(4);
});
```  
- `toBe` is one type of "matcher" that tests for exact equality with the value that is passed into it. 
	- There are [other types of matchers](https://jestjs.io/docs/using-matchers) that can be used to test for ranges, regular expressions, or compare objects, etc.
- Note that there is a function `it()` that is an alias for `test()`. They are [exactly the same](https://stackoverflow.com/questions/45778192/what-is-the-difference-between-it-and-test-in-jest).
- By default, the current version of Jest will not recognize ES6 import statements. You should use [[babel|Babel]] to transpile to ES5 ([specific instructions](https://jestjs.io/docs/en/getting-started#using-babel))
- [Instructions]([vscode-recipes/debugging-jest-tests at master · jherax/vscode-recipes (github.com)](https://github.com/jherax/vscode-recipes/tree/master/debugging-jest-tests)) for debugging jest tests in vs code.
- To run only one test with Jest, temporarily change that `test` command to a `test.only`. 
	- If a test is failing, one of the first things to check should be whether the test is failing when it's the only test that runs.

#### Next steps with Jest 
[Setup and Teardown · Jest (jestjs.io)](https://jestjs.io/docs/setup-teardown#general-advice)
[Mock Functions · Jest (jestjs.io)](https://jestjs.io/docs/mock-functions)
[Using with webpack · Jest (jestjs.io)](https://jestjs.io/docs/webpack)
