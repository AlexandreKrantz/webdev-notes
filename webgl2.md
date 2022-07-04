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

### Uniforms
- "Uniforms" are global variables. We define them in our shaders and set their value with javascript. A uniform value remains constant throughout a draw call.
	- You have to request a pointer (memory address) with `getUniformLocation()`
	- You then use `gl.uniform4f(pointer, data)`. 4f means that you are passing in a vec4. 
	- If you want to pass in an array of vec4, you use `gl.uniform4fv(pointer, data)`. You declare this uniform in your code as you would in C: `uniform vec4 myVarName[3]` (example with 3 elements).
- If you don't declare the value of a uniform in javascript, it gets a default value. 
	- For example an unset vec4 would be `[0, 0, 0, 1]`.
	- Default value for a sampler2D is `texture0`. 

### Attributes
- In math, a vertex is just a point in space. In webgl, a vertex can also have attributes like color info. 
- Attributes are only available in the vertex shader. Their value can change for each vertex in a draw call. 
- Declare an attribute in your vertex shader with `in`.
- You can forward attributes from the vertex shader to the fragment shader by declaring it as going `out` in the vertex shader. You then need to import it into the fragment shader using `in`.
- Each WebGL program is guaranteed 16 attributes. Using more is risky. 
- You can set the attribute memory location directly in your shader code, when you declare an attribute variable: `layout(location = 0) in vec2 anAttributeName`


### Objects, Targets & Binding
- Each object can have multiple targets. A target gives an object extra abilities. Until you bind an object to a target, many of the `gl` functions cannot be used. 
- Binding connects an object with a set of functions. 

### Textures
- Instead of setting `fragColor` in our fragment shader to a `vec4` color, we calculate the color using the `texture(uSampler, vTexCoord)`  function. 
	- sampler = "intellegent wrapper" for an image. Gives us an exact color for a precise coordinate (does interpolation if necessary).
	- texture coordinate = an [x,y] position point on the page. This coordinate goes from 0 to 1, where [0,0] is bottom-left and [1,1] is top-right.
- By default, WebGL expects you to use mipmaps. You need to override the default setting if mipmaps aren't needed: `gl.texParameteri(gl.TEXTURE_2D, gl.TEXTURE_MIN_FILTER, gl.NEAREST);`. This sets the image minimization function. 
- For most image files the origin is at the top, but for WebGL the origin is at the bottom. Therefore, we need to filp images so that they are not upside down. The easiest way to do this is to change the pixelStore setting: `gl.pixelStorei(gl.UNPACK_FLIP_Y_WEBGL, true);`
- 