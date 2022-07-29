- The raw primitives that WebAssembly adds to the Web platform = a binary format for code and APIs for loading and running this binary code. 
- By itself, WebAssembly cannot currently directly access the DOM; it can only call JavaScript, passing in integer and floating point primitive data types. Thus, to access any Web API, WebAssembly needs to call out to JavaScript, which then makes the Web API call. 
## Using Emscripten
- Emscripten generates an HTML file with the JS glue code. The JavaScript glue code is not as simple as you might imagine. For a start, Emscripten implements popular C/C++ libraries like SDL, OpenGL, etc. 
  - The generated HTML document loads the JavaScript glue file and writes stdout to a `<textarea>`. If the application uses OpenGL, the HTML also contains a `<canvas>` element that is used as the rendering target. 
- To use a custom HTML template:
```bash
emcc -o hello2.html hello2.c -O3 --shell-file html_template/shell_minimal.html
```
  - Path to the custom HTML template = `html_template/shell_minimal.html`. 
- By default, Emscripten-generated code always just calls the `main()` function, and other functions are eliminated as dead code. Putting `EMSCRIPTEN_KEEPALIVE` before a function name stops this from happening. You also need to import the `emscripten.h` library to use `EMSCRIPTEN_KEEPALIVE`.
  - You also need to compile with `NO_EXIT_RUNTIME`, which is necessary as otherwise when main() exits the runtime would be shut down.
  - Bash command to compile module: `emcc -o hello3.html hello3.c -O3 --shell-file html_template/shell_minimal.html -s NO_EXIT_RUNTIME=1 -s "EXPORTED_RUNTIME_METHODS=['ccall']"`
- To call the compiled C function
```javascript
var result = Module.ccall(
            'myFunction',  // name of C function
            null,  // return type
            null,  // argument types
            null  // arguments
        );
```

## Loading wasm with fetch()
- You can also make some wasm using WasmFiddle and then load the binary into your JS using fetch(): `const response = await fetch("simple.wasm");`
- Then, to instantiate a new WebAssembly.Module and WebAssembly.Instance: `const result = await WebAssembly.instantiateStreaming(response, importObject);`
  - Idk what the second argument actually means but this seems to work: `const importObject = { imports: { imported_func: arg => console.log(arg) } };`
Full example:
```javascript
WebAssembly.instantiateStreaming(fetch('simple.wasm'), importObject)
.then(obj => obj.instance.exports.exported_func());
```
- The net result of this is that we call our exported WebAssembly function exported_func, which in turn calls our imported JavaScript function imported_func, which logs the value provided inside the WebAssembly instance to the console. 
- For the non-streaming method, the API doesn't directly access byte code, so it require an extra step to turn the response into an ArrayBuffer:
```javascript
fetch('simple.wasm').then(response =>
  response.arrayBuffer()
).then(bytes =>
  WebAssembly.instantiate(bytes, importObject)
).then(results => {
  results.instance.exports.exported_func();
});
```
## ArrayBuffer and TypedArray
- `ArrayBuffer` object is used to represent a fixed length binary data segment. You can't directly manipulate its contents; to do that you need to create a `TypedArray`.
- To create an array buffer you can use the constructor and pass the number of bytes `new ArrayBuffer(numOfBytes)`. 
- Another way to create an ArrayBuffer is to call the `.arrayBuffer();` method that is part of fetch response objects and blob objects. 
- "TypedArray" doesn't represent an actual constructor, it is just the header term for a bunch of different constructors that have serve a similar functionality. 
  - When creating an instance of a TypedArray (e.g. `Int8Array`), an array buffer is created internally in memory or, if an ArrayBuffer object is given as constructor argument, then this is used instead. 
  - Typed arrays are like an extension to array buffers that allow interaction with the contents. 
- `Uint8Array` = unsigned array of 8 bit integers. `new Uint8Array(8)` instantiates an array of 8 elements, because the argument of the constructor is the length of the array (ArrayBuffer size created = length * bytes per element).
- You can reference elements in the array using standard array index syntax (that is, using bracket notation). Eg: `- int16[0] = 42;`
  - However, getting or setting indexed properties on typed arrays will not search in the prototype chain for this property, even when the indices are out of bound. Indexed properties will consult the ArrayBuffer and will never look at object properties. You can still use named properties, just like with all objects. 

## Memory
```javascript
var memory = new WebAssembly.Memory({initial:10, maximum:100});
```
- The unit of `initial` and `maximum` is WebAssembly pages — these are fixed to 64KB in size. 
