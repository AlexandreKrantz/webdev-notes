In a nutshell: Design by contract is about documenting the rights and responsibilities of software modules, to ensure correct utilisation and program correctness. 

Remove ambiguity by specifying the full range of acceptable arguments for a method. Do this in the comments/documentation. For example, to avoid people construcuting a `null` card. 
- A **precondition** is a predicate that must be true when a method starts executing.
	- typically involves the value of the method’s arguments, including the state of the target object upon which the method is called.
- **postconditions** are predicates that must be true when the execution of the method is completed.

- Specify the conditions in comments above the method definition with the `@pre` and `@post` tags. 
- You can check your conditions are met in the code with `if` statement or `assert`. 
	- The assert statement evaluates the predicate in the statement and raises an `AssertionError` if the predicate evaluates to false.
```java
class Card {
	Rank aRank;
	Suit aSuit;
	/** 
	* @pre pRank != null && pSuit != null 
	*/
	Card(Rank pRank, Suit pSuit) {
		assert pRank != null && pSuit != null; 
		aRank = pRank;
		aSuit = pSuit;
	}
	// other stuff...
}
```

- If a precondition check fails, the client (caller method) is to blame. If a postcondition check fails, the actual method being called is to blame.


When you check with if statements, you want to throw a specific exception: 
```java
	Card(Rank pRank, Suit pSuit) {
		if (pRank == null || pSuit == null) {
			throw new IllegalArgumentException("The argument cannot be null");
		} 
		aRank = pRank;
		aSuit = pSuit;
	}
```

### Public Interfaces
Objects provide a service. The terms of the service are specified in a public interface. 
- Requirements (preconditions)
- Modifications (will any objects be modified during the method call)
- Effects (will anything else happen due to the method call)

```java
/**
* @pre
* @post 
*/
```