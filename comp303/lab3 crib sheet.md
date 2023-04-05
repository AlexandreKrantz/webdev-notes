Coverage: Design for Robustness, Unit Testing, Composition

## Design for Robustness
- What should public interface specify? Note that in this case "interface" refers to a comment block that precedes the declaration of a class, data structure, or method. 
	- Information for using a class/method
		- For example: requires, modifies, effects
- Data fields = additional comments next to variables if the name/type doesn't provide sufficient info. 
- Implementation comments = comments inside a method, for a explaining the functionality of a specific piece of code. 

- What are the three aspects of design by contract? 
	- preconditions, postconditions, invariants

- When you check preconditions/postconditions, you should throw specific and explanatory errors.  [1] 
	- Only use the default assertions for non-public methods, where the client cannot check the input. 
Example of [1] : 
```java
if(Student == null){
	throw new NullPointerException();
}
```
- Write comments first. Capture the abstraction before the implementation. 

### Custom exception
Checked exceptions:
- indicated in method signature. Eg. `public void enroll(Student pStudent) throws CourseFullException`
	- Define the exception: `CourseFullException extends Exception`. 
```java
class myCustomException extends Exception { 
	myCustomException() {
		super();
	}

	// optional
	myCustomException(String msg) {
		super(msg);
	}
}
```
- Client is responsible for handling it (method should be called in a try/catch). 


REVIEW: creating exceptions + runtime vs. checked exceptions

- Checked exceptions: used for abnormal cases that can be recovered at runtime
	- Supplier needs to declare the exception in the method signature
	- Client needs to *catch* or *declare* the exception (in outer method signature)

- Unchecked exceptions: used for programming errors or things that should not happen at runtime. 
	- Code supplier does not have to declare it 
	- Code client does not have to catch or declare it

```java
public void enroll(Student pStudent) throws CourseFullException {
	if (pStudent == null ) {
		throw new NullPointerException();
	} else {
		throw new CourseFullException(); 
	}
}
```
	- Example of a checked exception method.
	- Client no longer has to check if the course is full before adding (but must handle the potential error in a try-catch block).


## Unit Testing
- The `Class` class
	- No constructor; automatically constructed by the VM. 
	- To get the `Class` object of an object: `obj.getClass()`
		- On this `Class` object, we can call methods such as `getName()`, `getDeclaredFields()`, etc. 
	- Reflection can be used to test private methods: [[Metaprogramming#Example]]
- How to write unit tests.
	- Public, non-static, methods. And remember the `@Test` annotation. 
		- AAA pattern for writing test method.
	- One test suite class per project class. 
- Generally, you don't need to test private methods. But if necessary you can:
	- Make a public helper method in the project class that tests the private method. 
- Sometimes, class relies on external classes/functionality. So it can't really be tested in isolation.
	- In this case, you want to use *stubs* to simulate the essential functionality of those external classes. 
- Define:
	- Efficient testing
	- Back box testing
	- White box testing
	- Structural testing
	- Functional testing
	- Statement coverage
	- Branch coverage
	- Path coverage

## Composition
- [[Command Design Pattern]]
	- Let's define commands as objects that implement the `Command` interface. 
		- The interface includes an `execute()` method. Which often takes another object as parameter, so it can execute on that object. 
		- Interface may also include other methods for managing the command, such as `undo()`, `getDescription()`, etc...
			- You may want to store data of the previous state in order to implement `undo`. 
			- You may need to access private parts of target object. You can do this by creating the Command object inside of the target class, via a `createCommand()` factory method.
		- All of these aforementioned interface methods have a return type of `void`.
- [[Composite Design Pattern]]
	- Composite object == aggregate object (ie. it contains other objects)
	- A **composite object** is an object that contains one or more other objects.
	- A **leaf object** is an object that has no children and represents an indivisible element of the hierarchy.
	- both composite and leaf objects should have a ***common interface*** that allows them to be treated in the same way. 
- Composition
	- To avoid God Classes, we must practice *delegation*. Certain isolated functionalities/services are delegated to a new, separate class. 
	- To represent composition in UML, use a white diamond decoration instead of an arrowhead. The "arrow" points to the class the stores the reference (aka the "parent").
- [[Decorator Design Pattern]]
	- Add additional functionality to an object, without affecting other objects of that class. 
	- Decorator class wraps stores reference to undecorated object
	- Decorator class implements the same interface as undecorated class, but changes up the functionality of the methods slighlty (while still using methods from the undecorated object). 
	- You create a decorated object by passing in the undecorated object as a parameter to the decorator's constructor.
- [[Law of Demeter]]
	- Only access the instance variables/methods of the implicit parameter. 
- [[Polymorphic Object Cloning#A Recipe for Cloning]]
	- Implement the `Cloneable` interface in the class
		- Override the `Object.clone()` method
			- When overriding clone, it is also recommend to change its return type from `Object` to the type of the class that contains the new clone method.
```java
public Deck clone() { 
	// NOT Deck clone = new Deck(); 
	Deck clone = (Deck) super.clone(); 
	// do stuff?
	return clone;
}
```
- The proper way to create the cloned object is by calling the method `clone()` defined in class `Object` by using the call `super.clone()`.
- To be safe, call `super.clone()` in a try catch to avoid potential `CloneNotSupportException` (shouldn't happen as long as you implement `Cloneable`). 

- [[Prototype Design Pattern]]
	- We can use [[Polymorphic Object Cloning]] to support *polymorphic instantiation*. 
	
	- Scenario: create new objects using a method, but the exact type is not known at compile time. 
	- Solution: store a reference to a "prototype" object and polymorphically clone this object to create new objects. 
	
	Here, the `CardSourceManager` object has a method for creating new `CardSource` objects of any type based on a prototype: 
```java
public class CardSourceManager { 
	private CardSource aPrototype = new Deck(); // Default 
	
	public void setPrototype( CardSource pPrototype ) { 
		aPrototype = pPrototype; 
	} 
	
	public CardSource createCardSource() { 
		return aPrototype.clone(); 
	} 
}
```

- [[Sequence Diagrams]]
For example if we want to call the `isEmpty()` method of a `CompositeCardSource`, then it is necessary to descend down all the aggregate objects and call their `isEmpty()` method also. 

Example: 
![[Pasted image 20230303184805.png]]
Solid arrows are method calls, the dashed arrows are responses. 

