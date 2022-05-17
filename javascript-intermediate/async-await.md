- When a function is declared with async, it automatically returns a promise; returning in an async function is the same as resolving a promise. 
- Throwing an error in the function will reject the promise.
- You can put a `.catch()` after an async function call to catch rejections. 
- You can also use try/catch blocks within async functions just like you would for synchronous code:
```javascript
async function getPersonsInfo(name) {
  try {
    const people = await server.getPeople();
    const person = people.find(person => { return person.name === name });
    return person;
  } catch (error) {
    // Handle the error any way you'd like
  }
}
```

