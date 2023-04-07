- Use abstract classes to avoid implementing duplicate methods in different classes. 
- Runtime type = initialized type of the object, unchanging throughout life of object. 
- compile/static type = type of the pointer to the object, can be downcasted to a supertype. 
- First call of any constructor is a call to `super()` => fields are initialized top down.
- If you override a method, the subclass version will become the default. To call the super class version, do `super.methodName();`.
	- `@Override` states to the compiler that the method is supposed to override another. 
	- `final` methods cannot be overriden.
- Inheritance is better if your system can be configured at compile time; composition is better if you want you system to support run-time modifications. 
	- `final` classes cannot be inherited
	- Inheritance should be used to (1) avoid duplicate code and (2) extend class with extra functionality. 

- **LSP:** If S is a subtype of T, then objects of type T may be replaced with objects of type S without altering any of the desirable properties of the program.

- **Template Method:** algorithm applies to a base class, but implementation varies based on the subclass. 
	- [[Template Method Design Pattern#Example]]

- **Observer Design Pattern:** observer interface, implemented by several observer (event listener) objects. Model class, that holds and arraylist of observers.
	- Whenever an observer object has an event, it notifies the Model, and the model then notifies the other observers of the new state. 
![[Pasted image 20230329092546.png]]
![[Pasted image 20230329093830.png]]
![[Pasted image 20230329094645.png]]

### visitor
- Create an interface for `CardSourceVisitor` objects that interact with `CardSource` objects. 
	- This is *abstract* visitor
```java
public interface CardSourceVisitor {
	void visitCompositeCardSource( CompositeCardSource pSource ); 
	void visitDeck( Deck pDeck ); 
	void visitCardSequence( CardSequence pCardSequence );
}
```
- A *concrete* visitor is an implementation of this interface. 
	- You can create different concrete visitors for each operation you want to do. 
```java
public class PrintingVisitor implements CardSourceVisitor { 
// you only need to implement the methods for the object relevant to the operation
	public void visitCompositeCardSource(CompositeCardSource pSource) {} 
	
	public void visitDeck(Deck pDeck) { 
		for( Card card : pDeck) { 
			System.out.println(card); 
		} 
	} 
	public void visitCardSequence(CardSequence pCardSequence) { 
		for( int i = 0; i < pCardSequence.size(); i++ ) { 
			System.out.println(pCardSequence.get(i)); 
		} 
	} 
}
```
- An interesting observation about the implementation of the concrete visitor is that it provides a way to **organize code in terms of functionality as opposed to data**. In a classic design, the code to implement the printing operation would be scattered throughout the three card source classes. 

![[Pasted image 20230329223139.png]]

