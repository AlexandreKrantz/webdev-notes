[[Observer Design Pattern]] is used often in GUI development.
- Framework code consists of a *component library* and an *application skeleton*
	- Components are buttons, and other UI elements
	- application skeleton connects your logic code to GUI, and takes care of monitoring events and input. 
- Application code is what gives the GUI the desired functionality. It's the behind the scenes logic stuff. 

- A GUI application doesn't execute sequentially from the entry point like a regular script-like application. 
	- Application is started by *launching* the framework using a special library method. 
	- The framework's application skeleton automatically launches the *event loop* which monitors user input devices. Throughout the execution of the GUI app, the framework remains in control and responsible for calling application code. 
	- This is an example of inversion of control because the code is not prompting the client; the client is prompting the application code. 
- Conceptually, the application code for a GUI application can be split into two categories: the *component graph*, and the *event handling code*.
	- Component graph is a tree of all the visible (eg. button) and invisible (eg. div) GUI elements. 
	- The framework will trigger events and UI changes when buttons are clicked on, but application code needs to *handle* the event in order for something to happen.

Drawing a UML component graph: 
- It can be useful to think about this user interface from four different point of views, or perspectives: user experience, logical, source code, run-time.

Event loop diagram:
![[Pasted image 20230329220146.png]]

Design pattern: event handler object. 

