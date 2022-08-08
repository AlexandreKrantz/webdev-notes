The Event Loop: 
- [Great video](https://www.youtube.com/watch?v=8aGhZQkoFbQ)
- V8 is the javascript runtime inside of chrome. It has a heap (runtime memory allocation) and a call-stack (functions that need to be executed).
- Asynchronous stuff like setTimeout is not in the V8 source.  It is a web API that is provided by the browser. The DOM is another web API, external from the V8 JS runtime.
- JS is a single threaded language => single call stack => only one piece of code can run at a time
- **Def. blocking** = code that is slow and on the stack (therefore it “blocks” the whole program). For browsers, this will freeze the whole UI/rendering. 
- But, the browser is more than just the runtime. In fact, [[apis|web APIs]] are like other threads which enable more than one thing to run at a time.
- When you call an async function (eg. setTimeout), you simply call the web API. The web API does the necessary computation concurrently (while you program continues to run). Then, when the API call is finished, it pushes onto the task queue. The event loop checks the call stack and the task queue. When there is something on the task queue and nothing on the call stack it pushes the first item on the task queue to the call stack. At that point, the async function can execute (eg. callback function for setTimeout)
- The javascript engine can’t repaint the page if there is code on the stack. You can think of it as the “render page” function being pushed to the task queue 60 times per second. Except it doesn’t get added to the regular callback queue, but instead to a special render queue that is higher priority. A “render queue” function is pushed to the render queue every 16ms and is pushed to the stack first. Only when the stack is empty does the event loop push from the regular task queue. 
- Note: on the backend, instead of web APIs you have C++ APIs

