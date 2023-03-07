Use a special type of object, compatible with polymorphism, to represent the null value (instead of an actually null value). 
For example, an interface that both regular cards and the `nullCard` implement . Both of them would implement an `isNull()` method. 

In Java, default methods are methods that have an implementation in the interface which is applicable to instances of all implementing types. You can use this to make the default `isNull` return false, so you won't need to override every implementation. 

To eleminate the need to implement a whole separate class for null, you could directly instantiate your `null` object in the interface:
```java
public interface CardSource { 
	public static CardSource NULL = new CardSource() { 
		public boolean isEmpty() { return true; } 
		public Card draw() { 
			assert !isEmpty(); 
			return null; // because calling draw on the NULL card source will automatically violate the precondition; the fact that we return a null reference afterwards is inconsequent.
		} 
		public boolean isNull() { return true; } 
	}; 
	Card draw(); 
	boolean isEmpty(); 
	default boolean isNull() { return false; } }
```
