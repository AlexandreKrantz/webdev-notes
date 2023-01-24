- Information hiding: Reveal the minimum amount of information for the required use case, and hide the rest. (eg. stack)
- Representing a card from a regular 4-suit deck could be done with an `int` ranging from 0-51 or a two-element int array (suit, number). 
	- These are both not great because the representation does not directly link to the corresponding concept. The representation links to the concept of an "integer", not a suit.
	- Also, other parts of the code will reference the card as an `int` or `int array`. So changing the type of the card will cause problems elsewhere. 
	- The int variables could take values outside of the intended range 0-51, and there's nothing we can do about it. 
- **Primitive obsession (antipattern):** Generally, it's a bad practice to shoehorn "domain concepts" into basic types.
	- `int` type should only be used to hold actual integers (and perhaps very similar concepts, such as currency). Similarly, Strings should be used only to hold sequences of characters meant to represent text or text-like information, as opposed to being some encoding of some other concept (e.g., "AceOfClubs").
	- **Solution =** hide the actual method of implementation, expose an interface tied to the concept of a card. 
```java
enum Suit { CLUBS, DIAMONDS, SPADES, HEARTS }
// ^ Suit is a type with 4 possible values: Suit.CLUBS, Suit.DIAMONDS, Suit.SPADES, ...

Suit suit1 = Suit.CLUBS; 
Suit suit2 = Suit.CLUBS; 
boolean same = suit1 == suit2; // same == true

enum Suit { CLUBS, DIAMONDS, SPADES, HEARTS } 
enum Rank { ACE, TWO, ..., QUEEN, KING } 
class Card { 
	Suit aSuit; 
	Rank aRank; 
}
```

We could use an ArrayList to represent the deck of cards, but we would run into the same problems: 
- list of cards is not strongly tied to the concept of a deck
- using the list of cards ties other parts of the software to this representation (would have to be updated in many places)
- structure could be easily corrupted since we can't restrict the length to 52. 
Better way is to define a new type: 
```java
class Deck { 
	List aCards = new ArrayList<>(); 
}
```
But even this, there are many way to misuse it and cause bugs because we still directly access the ArrayList. That's why we want to use encapsulation and hide the internal workings. 

You create a new **scope** in java by encapsulating some code in curly braces: 
```java
public static void main(String[] args) { 
	{ int a = 0; } // now the variable "a" cannot be accessed elsewhere. 
}
```
- general principle for achieving good encapsulation is to use the narrowest possible scope for class members. Thus, instance variables should almost always be private. 

```java
public class Card {  
// Made final so that it s immutable  
private final Suit aSuit;  
private final Rank aRank;  

public Card(Rank pRank, Suit pSuit) {  
	// Add stuff to avoid null references  
	aRank = pRank;  
	aSuit = pSuit;  
}  
public Rank getRank() {  
	return aRank;  
}  
// Might not want to have  
public void setRank(Rank pRank) {  
	aRank = pRank;  
}
```


### Design by Contract
Remove ambiguity by specifying the full range of acceptable arguments for a method. Do this in the comments/documentation. For example, to avoid people construcuting a `null` card. 
- A **precondition** is a predicate that must be true when a method starts executing.
	- typically involves the value of the method’s arguments, including the state of the target object upon which the method is called.
- **postconditions** are predicates that must be true when the execution of the method is completed.
- Specify the conditions in comments above the method definition with the `@pre` and `@post` tags. 
- You can check your conditions are met in the code with `if` statement or `assert`. 