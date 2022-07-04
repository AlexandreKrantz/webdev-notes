- Great tutorial series: https://www.youtube.com/watch?v=-T6EbWCq99c&list=PLPbmjY2NVO_X1U1JzLxLDdRn4NmtxyQQo&index=1

### Setting up a WebGL Program
1. Get the `gl` context.
2. Instantiate the program: `const program = gl.createProgram()`
3. For both the vertex and fragment shader you need to: create shader, set glsl source, compile shader, attach to webgl program.
4. Once both shaders are attached, you link the the program.
5. Now you can use the program: `gl.useProgram(program)`

### Shaders
- Almost all data types in WebGL have default precision values but the precision of *floats in fragment shaders* is not defined. So you need to explicitly define the precision of floats in your fragment shader source. 
	- Vectors and matricies are composed of floats and should also have their precisions defined. You can define the precision of a variable when declaring it: `out mediump vec4 myVarNam`.
	- Sampler variable types in both vertex and fragment shaders should have their precisions explicitly defined.
- "Uniforms" are global variables. We define them in our shaders and set their value with javascript. A uniform value remains constant throughout a draw call.
	- You have to request a pointer (memory address) with `getUniformLocation()`
	- You then use `gl.uniform4f(pointer, data)`. 4f means that you are passing in a vec4. 
	- If you want to pass in an array of vec4, you use `gl.uniform4fv(pointer, data)`. You declare this uniform in your code as you would in C: `uniform vec4 myVarName[3]` (example with 3 elements).

### Attributes
- In math, a vertex is just a point in space. In webgl, a vertex can also have attributes like color info. 
- Attributes are only available in the vertex shader. Their value can change for each vertex in a draw call. 
- Declare an attribute in your vertex shader with `in`.
- You can forward attributes from the vertex shader to the fragment shader by declaring it as going `out` in the vertex shader. You then need to import it into the fragment shader using `in`.
- Each WebGL program is guaranteed 16 attributes. Using more is risky. 
- You have to set attributes in a similar way to uniforms. Use `gl.getAttributeLocation`
and 