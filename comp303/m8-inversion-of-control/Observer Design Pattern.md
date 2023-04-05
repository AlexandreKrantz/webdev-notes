An implementation of the [[Inversion of Control#Mode-View-Controller Decomposition (MVC)|Model-View-Controller]] abstraction. 

### Model
Store the data in an a specialized object that allows other objects to observe it easily. We call this object the *subject*, *model*, or *observable*. 
![[Pasted image 20230329092546.png]]
The Model object stores the data. It also stores a collection of observer instances. 
- This is loose coupling; Model can still function with an empty list of observers, and observers can be added at runtime. 

### Callback Method
Whenever there is a change in the model’s state worth reporting to observers, the model should let the observers know by cycling through the list of observers and calling a certain method on them.
- This method is defined on the `Observer` interface. 
	- It's an example of a "callback" method that inverts the control flow. To find out information from the model **the observers do not call a method on the model, they instead “wait” for the model to call them (back).**
	- The idea of a callback is not to tell observers what to do, but rather to inform observers about some change in the model, and let them deal with it as they see fit (through the logic provided in the callback).
- It is a good practice to name callback methods with a name that describes a state-change situation, as opposed to a command. 
	- For example `myObserverObj.newNumber(n)` indicates that the model has a new number. 
	- `myObserver.setNumber(n)` would be bad practice. 

### Notification Method
The model object has **notification method** which calls the callback of each observer in the list. We need to make sure this method is called whenever there is a state change. 
![[Pasted image 20230329093830.png]]
- Method can't be called `notify()` as this is the name of a legacy Object method. 

When to call?
1. Any state-changing method should call the notification method. 
2. Only call when it's necessary to inform observers (if doing it on every change cause performance problems).
Eg: call to `notifyObservers()` in the `setNumber(n)` method. Each observer then reacts to the `newNumber(n)` method by making corresponding changes to GUI. 
![[Pasted image 20230329094645.png]]

- Push data-flow strategy: Sending information to observers via the arguments of the callback function is called "pushing" data. 
	- The type of the data is pre-determined. 
- There may be cases where we want to the type of the data to be flexible and determined at runtime. In this case it's better to use a "pull" data-flow strategy, where you send the whole model object as a parameter to `newNumber(m)` and let `newNumber` pick out any (publicly available) data. 
- the reference to `Model` doesn't have to be provided as an argument to the callback method. Another option is to initialize observer objects with a reference to the model (stored as a field), and simply refer to that field as necessary.
	- For example: 
![[Pasted image 20230329100314.png]]

This does introduce pretty tightly coupling between our specific model object and the observers. We can reduce the coupling with the Interface Segregation Principle:
![[Pasted image 20230329100539.png]]

Supporting both strategies is can also be good for increasing the reusability of the `Observer` interface:
![[Pasted image 20230329101223.png]]

- Sometimes you don't even need to send data, and just having a callback with no parameters will do. 
- The return value of the callback should always be `void`. 

### Event-Based Programming
- One way to think about callback methods is as events, with the model being the event source and the observers being the event handlers. 
- The model generates a series of events that correspond to different state changes of interest, and other objects are in charge of reacting to these events.

You can make different callback to correspond to different events:
![[Pasted image 20230329102325.png]]
- If a specific observer is only interested when the number increases, for example, it only needs to react to that event (empty methods for the others). 
- We assume the events are mutually exclusive (we can make separate events to capture intersectionality). 
![[Pasted image 20230329102555.png]]

We could also split Observers into different categories with different interfaces, to avoid too many do-nothing methods:
![[Pasted image 20230329102857.png]]
With two abstract observers, concrete observers can be more targeted and only register for the sets of events they need to respond to. The trade-off for more flexibility is a slightly heavier interface for the Model class, because it now has to support two lists of observers with their corresponding registration methods.
