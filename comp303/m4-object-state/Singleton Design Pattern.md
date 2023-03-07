Aim: ensure that there is only one instance of a given class at any point in the execution of the code.

1. A private constructor for the singleton class, so clients cannot create duplicate objects;
2. A global variable for holding a reference to the single instance of the singleton object
3. A static accessor method, usually called instance(), that returns the singleton instance.

```java
public class GameModel { 
	// variable that holds the refernce to the single instance
	private static final GameModel INSTANCE = new GameModel(); 
	
	// private constructor
	private GameModel() { ... } 
	
	// so that the client can access the reference to the single instance. 
	public static GameModel instance() { return INSTANCE; } 
}
```

The SINGLETON pattern differs from FLYWEIGHT in that it attempts to guarantee that there is a single instance of a class, as opposed to unique instances of a class. Singleton objects are typically stateful and mutable, whereas flyweight objects should be immutable.
