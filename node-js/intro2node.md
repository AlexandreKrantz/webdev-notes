## What is NodeJS?
- Node = asynchronous event-driven Javascript runtime.
  - In this context asynchronous means that when you write your code you do not try to predict the exact sequence in which every line will run. Instead you write your code as a collection of smaller functions that get called in response to specific events such as a network request (event driven).
  - This means that multiple processes can run at the same time.
  - When a function finishes, it's an event that can trigger other functions to run. 
- Def. callbacks = functions that are passed into another function as an argument. 
- Both your browser JavaScript and Node.js run on the Chrome V8 JavaScript runtime engine. The engine essentially acts like an interpreter and compiler.
- `require()` is a function in Node used to load modules. These can be your own files, default bundled modules like file system and HTTP, or third-party npm modules.
  - A Node module is a reusable block of code whose existence does not accidentally impact other code.  
### Intro to the Server Side
- Web browsers communicate with web servers using HTTP. The HTTP request includes a URL identifying the affected resource, a method that defines the required action (for example to get, delete, or post the resource), and may include additional information encoded in URL parameters (the field-value pairs sent via a query string), as POST data (data sent by the HTTP POST method), or in associated cookies.
- Web servers wait for client request messages, process them when they arrive, and reply to the web browser with an HTTP response message. 
  - The response contains a status line indicating whether or not the request succeeded (e.g. "HTTP/1.1 200 OK" for success).
  - The body of a successful response to a request would contain the requested resource (e.g. a new HTML page, or an image, etc...), which could then be displayed by the web browser.
- Def. a static site is one that returns the same hard-coded content from the server whenever a particular resource is requested
![basic_static_app_server](img/basic_static_app_server.png)
- A dynamic website is one where some of the response content is generated dynamically, only when needed. 
  - Eg:  HTML pages created by inserting data from a database into placeholders in HTML templates 
![web_application_with_html_and_steps](img/web_application_with_html_and_steps.png)
- Server-side website programming mostly involves choosing which content is returned to the browser in response to requests. As long as the web app returns valid HTTP responses, it is free to be in any language and use all the features of the OS.
- Implementing a vital feature like an HTTP server is really hard to do from scratch, so you use frameworks (eg. Django, ExpressJS, etc)
### Client-Server Overview 
- HTTP request includes:
  - A URL identifying the target server and resource (e.g. an HTML file, a particular data point on the server, or a tool to run).
  - A method that defines the required action (for example, to get a file or to save or update some data). Two common ones:
    - `GET` = Get a specific resource (e.g. an HTML file)
    - `PUT` = Update an existing resource (or create a new one if it doesn't exist).
  - Additional information can be encoded with the request (for example, HTML form data). 
    - For `GET` requests, data is encoded with URL parameters (Eg. `http://example.com?name=Fred&age=11`)
    - For `POST` requests, data is encoded in the request body (more secure).
  - Client-side cookies
- HTTP Response includes:
  - Status code string: (e.g. `"200 OK"` for success, `"404 Not Found"` if the resource cannot be found, `"403 Forbidden"` if the user isn't authorized to see the resource, etc).
  - The requested resource (for `GET` requests)
- When an HTML page is returned it is rendered by the web browser. As part of processing the browser may discover links to other resources (e.g. an HTML page usually references JavaScript and CSS pages), and will send separate HTTP Requests to download these files.

## Node Basics
### How to Run Node.js Scripts
- `node app.js` to run it once.
- `nodemon app.js` to re-execute whenever there is a change made.
- `#!/usr/bin/env node` - add this to the beginning of your script to make the file directly executable. 

### Environment Variables
- Access environment variables within node script: `process.env.VARNAME;"`. `process` does not need to be imported, it's automatically available.

### Building an HTTP Server
- [Fantastic article](https://www.digitalocean.com/community/tutorials/how-to-create-a-web-server-in-node-js-with-the-http-module)

### File System
- `const fs = require('fs');`
- Pretty much any file operation that you can do with bash, you can do with fs. 
- All fs methods are asynchronous by default, but you can make them synchronous by appending `Sync` to the method name.
  - Using synchronous option will block the execution of the rest of your script while the operation completes. 
- Node.js 10 includes experimental support for a promise based API: `const fs = require('fs/promises')`
- Write file: `fs.writeFile('/Users/joe/test.txt', content);`, where content contains the string that you want to write. 
- Read the full data of a file: `const data = await fs.readFile('/Users/joe/test.txt', { encoding: 'utf8' });`
  - if a file is too big and reading it is affecting performance, you may want to try using fs streams. 


### Intro to NPM
- The difference between devDependencies and dependencies is that the former contains development tools, like a testing library, while the latter is bundled with the app in production.
- Add to dependencies with `-S` flag, to dev dependencies with `-D` flag, and to optional dependencies with `-O` flag. 
- update packages with `npm update`
- Specify scripts in your package.json:
```
{
  "scripts": {
    "start-dev": "node lib/server-development",
    "start": "node lib/server-production"
  }
}
```  
  - run the scripts with `npm run script-name`.
- The Semantic Versioning concept is simple: all versions have 3 digits: `x.y.z` (corresponding to major version, minor version, patch version).
  - you up the major version when you make incompatible API changes
  - you up the minor version when you add functionality in a backward-compatible manner
  - you up the patch version when you make backward-compatible bug fixes
- To uninstall a package you have previously installed locally: `npm uninstall <package-name>`
  - If the package is installed globally: `npm uninstall -g <package-name>`
- `npm uninstall` by itself doesn't update the `package.json`. You need to pass a flag like `npm uninstall -D webpack` to update the dependencies. 
- In your code you can only require *local* packages. So in general, all packages should be installed locally.
- A package should be installed globally when it provides an executable command that you run from the shell (CLI), and it's reused across projects. Check your global packages by running `npm list -g --depth 0`

### The URL Class
- available globally. Create a new URL object with `const myURL = new URL('/foo', 'https://example.org/');`
  - first argument is the input, which can be a relative or absolute url. If it is relative, then the second argument should contain the base url. 

### Events
- The events module - `require('events')` - provides the EventEmitter class. 
  - `const emitter = new EventEmitter();`
- `emitter.emit` emits an event. It synchronously calls every event listener in the order they were registered.