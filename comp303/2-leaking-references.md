#### leaking references
```java
public class Deck { 
	private List aCards = new ArrayList<>(); 
	public List getCards() { return aCards; } 
} // here when we call getCards, we leak the reference to our ArrayList, so it is no longer encapsulated
```

Solution: Add access methods that only return references to immutable objects
```java
public class Deck { 
	private List aCards = new ArrayList<>(); 
	public List getCards() { 
	      // Create a copy so that user can’t modify original 
	      return new ArrayList<> (aCards); 
	}
}
```

Shallow vs deep copy: Shallow saves the reference of deeper objects, deep copy doesn’t (creates new objects for everything).

Immutability: it should not be possible to modify the internal state of an object without going through its methods. 
	- Although sometimes you can return a reference, the type should be immutable so that even if you reassign the reference it has to be of the right type. 