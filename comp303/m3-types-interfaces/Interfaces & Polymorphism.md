-   In Interface does not have access modifiers. Everything defined inside the Interface is assumed to have a public modifier, whereas Abstract Class can have an access modifier.
-   The Interface cannot contain data fields, whereas the abstract class can have data fields.


```java
public interface CardSource { 
	public Card drawCard();
	public boolean isEmpty();
}

public class Deck implements CardSource {...}
```
- This enables polymorphism: A CardSource can take many shapes, one of them being a Deck. 

```java
// all valid
CardSource source = new Deck();

List list = new ArrayList<>();
```
- Here List is an interface that specifies the obvious services (add, remove, etc.), and ArrayList is a concrete implementation of this service that implements the list using an array structure. But we can just as well replace ArrayList with LinkedList and the code will still compile.

### Separation of Concerns
- Using small interfaces encourages the respect of a software design principle called separation of concerns
- A class can implement unlimited interfaces, so you might as well make separate interfaces each focused on a small piece of functionality. 
