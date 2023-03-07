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

- Advantage, we don't have to do branching or have a specified number of options for the class of the object we want to create. Everything is extensible and based on the prototype that can be set at runtime. 
- Potential drawback: You need to override the clone method for all the subclasses of the porotype which might not be easy to achieve