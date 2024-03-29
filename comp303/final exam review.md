Todo: github exercises 
### m6 Composition
- [[Command Design Pattern]] + example
- [[Composite Design Pattern]] + example
	- Composite vs. leaf object
	- Common interface
	- Aggregate object
- [[Decorator Design Pattern]]
	- [[Decorator Design Pattern#Combining Composite and Decorator |Combining Composite and Decorator]]
- [[Law of Demeter]]
	- Delegation chains antipattern
	- Law of detemeter
- [[Polymorphic Object Cloning]]
	- Copy constructure pros/cons
	- `Cloneable` interface
- [[Prototype Design Pattern]]
	- polymorphic object instantiation
- Object State Diagrams vs. [[Sequence Diagrams]]

### m7 Inheritance
- Why do we use inheritance? What is the conceptual relationship between superclass and subclass?
	- To avoid duplicated code. The superclass is a *generalization* of the subclass (white solid arrow). 
- Runtime vs. Static Type: 
	- Runtime = type of the object at initialization
	- Static (Compile time) = type of the pointer to an object  
	- Which is retained when passing an object into a method?
		- Static type
- How are the fields of inheriting objects intialized?
	- Top-down. So the first call of constructor should be to super. 
- How to use superclass methods // overriding?
	- A child class reimplemention of a superclass method is considered more specific, and thus *overrides* the superclass method. 
	- `super.methodName()` if method has been overridden. Otherwise, the superclass method is called by default. 
	- `@Override` is use to state that the following method is supposed to override another. if a method annotated with `@Override` does not actually override anything, a compilation error is raised.
- Overloading methods
	- Most specific signature is applied (only reads *static* type of arguments)
- `protected` vs. `private` 
	- The protected access modifier is accessible within the package. However, it can also accessible outside the package but **through inheritance only**.
	- Subclasses cannot access private fields of their superclass, but those fields still exist in the object instantiation.
- `final ____` + when to use
	- Means that method cannot be overridden by subclasses.
	- Means that class cannot be inherited
	- The use of `final` with fields limits how we can assign values to variables, and does not involve inheritance.
	- Use in the [[Template Method Design Pattern]] to ensure template is respecte in subclasses
- Use inhertiance to make an abstraction for Decorator Design Pattern
- [[Liskov Substitution Principle]]
- [[Template Method Design Pattern]]

### m8 Inversion of Control
- MVC decomposition: [MVC - MDN Web Docs Glossary](https://developer.mozilla.org/en-US/docs/Glossary/MVC)
- Observer pattern: What methods does the Model object have? What is the sequence diagram for a change in state? 
	- [[Observer Design Pattern]]
	- [[Observer Design Pattern#Push vs. Pull Data-Flow|Push vs. Pull Data-Flow]]
	- [[Observer Design Pattern#Event-Based Programming|Multiple Different Events]]
- [[Visitor Design Pattern]]
	- Why use it?
		- Say you have a bunch of classes that implement an interface, and you want to add extra funcitonality without polluting the interface. You can make a visitor object that acts on objects of those classes. 
	- How to use it? 
		- The visitor object will have a different method for each specific class implementation, that provides the wanted functionality. The class implementations will have `accept` methods that take in the visitor and trigger the correct visitation method. 
	- Two ways to traverse composite object
		- Via the accept method, define a traversal pattern and then make the visitor aggregated object. 
		- Make a composite object iterable, so the visitor can interate over it directly. 

### m9 Concurrency
- [[Concurrency in Java]]
	- def. process
	- def. thread
	- heap memory vs. stack memory
- Two methods to create a thread 
	- Implementing `Runnable`
	- Extending `Thread`
- What does sleeping a thread do? 
	- *Sleeping a thread will give up its slot on the CPU.* 
		- A sleeping thread is in a "blocked" state. 
		- When sleep period is over, thread is in a "ready" state. 
	- Other threads may be scheduled by the OS to run on the slot during its sleep time, and this could potentially block the thread from running for some additional time after its sleep period is over.
		- A thread in ready state may have to wait for other threads in the queue to finish. 
- Interrupting a thread
- Thread safety - why? 
	- Immutable objects (stateless objects)
	- Atomic operations (def. race conditions)
	- Synchronization (aka Locking)
		- *Serialization of resources* with `synchronized` modifier
	- `volatile` variables
	- Intrinsic locks