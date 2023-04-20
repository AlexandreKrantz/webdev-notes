# Visitor Design Pattern
- Another example of inverting control.
- Reduces object coupling. 
- Used when you want to support an open-ended number of operations that can be applied to an object graph, without needing to change to interface of the objects. 

### Scenario
Say you have several objects implementing `CardSource` interface
![[Pasted image 20230329220902.png]]
What if we want to extend the functionality of card sources, without breaking current code and bloating the interface with more methods? In this case we can use the Visitor pattern. 
=> we can add this extra functionality in a visitor object. 

### Example
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

### Integrating Operations into a Class Hierarchy
- Although a concrete visitor isolates a certain functionality in its class, it should still be integrated in the class hierarchy of the target object. 
- In the target object interface (eg. `CardSource`), define an `accept` method with a visitor parameter:
![[Pasted image 20230329223139.png]]

Process for visiting CardSource objects:
```java
PrintingVisitor visitor = new PrintingVisitor();
Deck deck = new Deck();
deck.accept(visitor);
```
Now the `accept` method can choose the circumstances under which it is visited (how/when). 

Now the `CardSource` interface is linked via dependency to `CardSourceVisitor`
![[Pasted image 20230329223457.png]]

### Traversing the Object Graph
Any object graph with more than one element will have an aggregate node as its root. In our case this is `CompositeCardSource`
```java
class CompositeCardSource implements CardSource {
	...
	public void accept (CardSourceVisitor pVisitor) {
		pVisitor.visitCompositeCardSource(this); // right now, this method does nothing. 
	}
}
```
This `CompositeCardSource` also contains a list of other `CardSource` objects. For example:
![[Pasted image 20230418215125.png]]

Two ways to traverse the object graph:
1. via the `accept` method
```java
public class CompositeCardSource implements CardSource { 
	private final List aElements; 
	public void accept(CardSourceVisitor pVisitor) { 
		pVisitor.visitCompositeCardSource(this); 
		for( CardSource source : aElements ) { 
			source.accept(pVisitor); 
		} 
	} 
}
```
- This is the more straightforward solution; we have access to the CardSources within the CompositeCardSource class. However this means that the visitor cannot determine the traversal order. 

2. via the `visitCompositeCardSource` method
	- If we implement `Iterable` in the composite object, we can traverse it from the visitor object.
```java
public class CompositeCardSource implements CardSource, Iterable { 
	private final List aElements; 
	public Iterator iterator() { 
		return aElements.iterator(); 
	} 
	... 
}

public class PrintVisitor implements CardSourceVisitor { 
	public void visitCompositeCardSource( CompositeCardSource pCompositeCardSource) 
	{ 
		for( CardSource source : pCompositeCardSource ) { 
		source.accept(this); 
		} 
	} 
	... 
}
```

#### Abstract Visitor Class
NOTE: If we place the traversal code in the visit methods, and have more than one concrete visitor class, every class is bound to repeat the traversal code in its visit method.

Solution = abstract visitor class that implements the `visitCompositeCardSource` method. 

### Supporting Data Flow in Visitor Structures
- The example of `PrintVisitor` doesn't have any input or output, but most operations *do* involve data flow. 
	- But the pattern requires that no assumption be made about the nature of the input and output of operations.
- Solution: store data within the visitor object. 
	- Input is provided when constructing the object.
	- Output can be accessed via getter methods. 
Example (outputting an int):
![[Pasted image 20230329225512.png]]

