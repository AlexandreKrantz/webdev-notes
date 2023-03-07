A *copy constructor*, built into the class itself, is an easy way to copy an object:
```java
Deck deckCopy = new Deck(pDeck);
```

However, it's possible that we want to copy a group of objects that all implement the same interface but aren't necessarily from the same class. In this case, we won't know which constructor to call: 
```java
public class CardSourceManager { 
	private final List<CardSource> aSources; 
	public List<CardSource> getSources() { 
		// return a copy of aSources; 
	} 
}
```
- In this example, all the list items implement CardSource but could potentially be a Deck, a Decorator etc. 

We could use a if/else block to try and identify the class but this would violate design principles for extensibility. 

=> copy constructors are incompatible with polymorphism. 
We need a new mechanism...

## A Recipe for Cloning
1. Implement the `Cloneable` interface in the class. 
	- check if the obj is cloneable: `if( o instanceof Cloneable ) { clone = o.clone(); }`
2. Override the `Object.clone()` method
	- When overriding clone, it is also recommend to change its return type from `Object` to the type of the class that contains the new clone method.
3. Calling `super.clone()` in the `clone()` method; 
	- The overridden clone method needs to create a new object of the same class. You don't want to use the class constructor for this. The proper way to create the cloned object is by calling the method `clone()` defined in class `Object` by using the call `super.clone()`.
```java
public Deck clone() { 
	// NOT Deck clone = new Deck(); 
	Deck clone = (Deck) super.clone(); 
	... 
}
```
4. Catching `CloneNotSupportedException` in the `clone()` method; 
	- If you implement `Cloneable`, you shouldn't get an error. 
	- But just to be safe, call `super.clone()` within a try/catch block. 
5. Optionally, declaring the `clone()` method in the root supertype of a cloneable hierarchy
	- Unfortunately (and strangely), the Cloneable interface does not include the `clone()` method. You want to add it yourself to the `CardSource` composite object interface in order to freely call `clone()` using polymorphism. 