# Polymorphic Object Cloning
## Copy Constructor
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

## Object Clone Method
Another way to copy an object is using the `clone()` method:  
`if( o instanceof Cloneable ) { clone = o.clone(); }`

If `o` is not already cloneable:
1. Implement the `Cloneable` interface
2. Override the `Object.clone()` method. There is a specific way you want to do this:
	- In your overriding method, call `super.clone()` and then downcast it to the current class. 
```java 
// example:
public Deck clone() { 
	Deck clone = (Deck) super.clone(); 
}
```

**Note:**
- Catching `CloneNotSupportedException` in the `clone()` method; 
	- If you implement `Cloneable`, you shouldn't get an error. 
	- But just to be safe, call `super.clone()` within a try/catch block. 
- Optionally, declaring the `clone()` method in the root supertype of a cloneable hierarchy
	- Unfortunately (and strangely), the Cloneable interface does not include the `clone()` method. You want to add it yourself to the `CardSource` composite object interface in order to freely call `clone()` using polymorphism. 

```java
public interface CardSource {
	...
	public CardSource clone();
}

public class Deck implements CardSource {
	...
	public Deck clone() throws CloneNotSupportedException {
		Deck clone = (Deck) super.clone(); 
	}
}
```

### Another Example
```java
// Java program to illustrate Cloneable interface
import java.lang.Cloneable;

// By implementing Cloneable interface
// we make sure that instances of class A
// can be cloned.
class A implements Cloneable {
	int i;
	String s;
	// A class constructor
	public A(int i, String s)
	{
		this.i = i;
		this.s = s;
	}
	// Overriding clone() method
	// by simply calling Object class
	// clone() method.
	@Override
	protected Object clone() throws CloneNotSupportedException
	{
		return super.clone();
	}
}

public class Test {
	public static void main(String[] args) throws CloneNotSupportedException
	{
		A a = new A(20, "GeeksForGeeks");
		// cloning 'a' and holding
		// new cloned object reference in b
		// down-casting as clone() return type is Object
		A b = (A)a.clone();
		System.out.println(b.i);
		System.out.println(b.s);
	}
}

```