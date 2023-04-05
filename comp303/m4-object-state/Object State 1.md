### Intro
Ways to view a software system: 
- in terms of elements declared in source code and relationships between them (aka. static, compile-time perspective) 
- in terms of the (initialized) objects currently running in the system (aka. dynamic, runtime)
	- What are all the values and references held by variables?

### Object States
1.  Concrete State:
	- When an object has a fixed set of attributes with defined values, it is said to have a concrete state. This means that the values of its attributes are known and can be directly accessed and modified. For example, consider a Car object with attributes such as make, model, year, color, and mileage. These attributes have specific values that define the state of the Car object.
2.  Abstract State:
	- On the other hand, an object can have an abstract state if its attributes are not fixed or if they have values that are not directly accessible or modifiable. In this case, the object may have attributes that are computed on the fly or derived from other attributes. An object with an abstract state can be thought of as having a more dynamic and flexible state than an object with a concrete state. For example, consider a BankAccount object with attributes such as balance, interest rate, and transaction history. The balance attribute may change frequently due to deposits and withdrawals, and the interest rate may be computed based on external factors such as market conditions.



- We usually refer to the set of possible states for a variable or object as its ***state space***.
	- With class Player, adding an aName field of type String blows up the size of the state space to something that is only limited by the physical memory of the computing system. For this reason, when designing software, it is more practical to think in terms of abstract states.

- Note that some object, like functions, do not have any associated state. They are ***stateless objects*** (as opposed to stateful). Immutable objects also only have a single state. 
	- This chapter is concerned with objects that are both **mutable** and **stateful**.


### Object State Diagrams
- It's a common mistake to use object state diagrams for modeling "data flow". 
- *If the names of your state are verbs or describe actions, that is a bad sign*. 
- Method calls will result in an object transitioning to different abstract states. This is the "life cycle" of the object. 
	- Good practice is to simplify the life cycle and minimize the state space of objects. 
		- It should be impossible to put the object in a useless state. For example, there is no use for an unshuffled deck of cards, so just make the initialization and shuffling of the deck part of the same method. You want to avoid *speculative generality*. 

- Information should not be stored in an object field unless it uniquely contributes to the intrinsic value represented by the object. 
	- A violation of this principle often constitutes an instance of the **TEMPORARY FIELD†** antipattern.



### Nullability
- Recommended best practice is to design classes so that null references are simply not used. All reference variables *should have a value*. 
- We can ensure that variables are not assigned a null reference by using either one of two approaches: input validation, or design by contract.
	- input validation = check inputs with an if statement
	- by contract = specify preconditions and check with `assert`

Java optional types: 
• A container object which may or may not contain a non-null value. 
• If a value is present, `isPresent()` will return true and `get()` will return the value
```java
public class Card { 
	private Optional aRank;
	private Optional aSuit; 
	private boolean aIsJoker
	
	public Card(Rank pRank, Suit pSuit)
	{
		assert pRank != null && pSuit != null;	
		aRank = Optional.of(pRank);
		aSuit = Optional.of(pSuit); // create full optional
	}
	public Card() {
		aIsJoker = true;
		aRank = Optional.empty(); // empty optional obj
		aSuit = Optional.empty();
	}
	
	public getRank () {
		if (aRank.isPresent()) { // check if optional is full
			Rank rank = aRank.get() // get the optinal value 
			return rank; 
		}
	}
}
```

### [[Null Object Design Pattern]]

### Final Fields and Variables
Recall: it's good practice to minimize the number of states an object can take
=> Because object state is just an abstraction of the combination of values taken by the fields of an object, the way to realize the principle in practice is to limit the number of ways in which the field values can be updated.
- The `final` keyword in java prevents a variable from being updated after its first initialization. 
	- Note that if a reference variable is final, we can still make modifications to the object (just not change out the object itself)

### Object Identity, Equality, Uniqueness
- Identity = unique memory location of object (often abstracted by IDEs to an id number)
- To compare ids of objects use `==`. To compare the class or values use `.getClass()` or `.equals()`
	- The default implementation of the `equals` method defines equality as identity; it compares the hashcodes of the two objects.
		 - If you want to override this method, you must also override hashCode method so that calling `.hashCode()` on each object produces the same result. 
			 - => `.equals()`  is a *more rigorous* check for equality than `==`
		 - The `equals` method should also respect the properties of a equivalence relation
- 
