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
[Fantastic article](https://www.digitalocean.com/community/tutorials/how-to-create-a-web-server-in-node-js-with-the-http-module)