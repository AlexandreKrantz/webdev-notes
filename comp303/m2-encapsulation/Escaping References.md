**Example: leaking references**
```java
public class Deck { 
	private List<Card> aCards = new ArrayList<Card>(); 
	public List getCards() { return aCards; } 
} // here when we call getCards, we leak the reference to our ArrayList, so it is no longer encapsulated
```

**Solution: Add access methods that only return references to immutable objects**
```java
public class Deck { 
	private List<Card> aCards = new ArrayList<Card>(); 
	public List getCards() { 
	      // Create a copy so that user can’t modify original 
	      return new ArrayList<Card> (aCards); 
	}
}
```

Shallow vs deep copy: Shallow saves the reference of deeper objects, deep copy doesn’t (creates new objects for everything).

Immutability: it should not be possible to modify the internal state of an object without going through its methods. 
	- Although sometimes you can return a reference, the type should be immutable so that even if you reassign the reference it has to be of the right type. 

**Example: receiving references**
```java
public class Deck { 
	private List aCards = new ArrayList<Card>(); 
	public void setCards(List<Card> pCards) {
		// make a new list, otherwise the reference will leaked and available outside
		aCards = new ArrayList<Card>(pCards); 
	}
	// other methods..
}
```

### Immutibility
- NOTE: When the object is immutable, it's okay to leak the reference (eg. with strings). 

Example: Immutable `Card` class
```java
public class Card { // impossible to change any attributes after initialization!
	private Rank aRank; 
	private Suit aSuit; 
	
	public Card(Rank pRank, Suit pSuit) { 
		aRank = pRank; 
		aSuit = pSuit; 
	} 
	
	public Rank getRank() { return aRank; } 
	public Suit getSuit() { return aSuit; } }
}
```

If Card is immutible, then we can return it directly in our deck class
```java
public class Deck { 
	private List aCards = new ArrayList<Card>(); 
	public void getCard(int index) {
		return aCards.get(index);
	}
	// other methods..
}
```