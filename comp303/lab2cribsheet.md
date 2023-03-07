Strategy in a nutshell:
- interface
	- classes that implement different versions of the behavior
- classes for the different "objects"
	 - object has a field of of type behaviorInterface
	 - the field is initialized in the constructor
	 - the methods of the class can use this behavior object

Separation of Concerns:
- Using small interfaces encourages the respect of a software design principle called separation of concerns
- A class can implement unlimited interfaces, so you might as well make separate interfaces each focused on a small piece of functionality. 

Flyweight:
- share objects immutable parts of objects that are identical, instead of creating new instances. 
- Say you have an object. You want to separate the intrinsic state (immutable properties) of an object from its extrinsic state (mutable properties), and to share the intrinsic state among multiple objects.
- Components:
	- Have a `private static final` map/arrary in the class where you store your instances
	- Have a static factory/generator/getObject function. This essentially acts like a constructor. It checks if an instance with the specified parameters isn't alread present in the map. If it is, it returns a reference to this object. If not, it initializes a new one and stores it in the map. 
- Example:  [[Flyweight Design Pattern#Example (ChatGPT)]]

- Iterator design pattern = using a java iterator. 
- [[Interator Design Pattern#Benefits]]

Nested classes:
- Within an inner class, you can access the current instance of the outer class with the following syntax: `enclosingClassName.this`
- Java also allows the declaration of `static` inner classes. Main difference: static nested classes are not linked to an outer instance.

[[Null Object Design Pattern]]:
- Use a special type of object, compatible with polymorphism, to represent the null value (instead of an actually null value). 
- For example, an interface that both regular cards and the `nullCard` implement . Both of them would implement an `isNull()` method. 
- This way you can make sure that actual `null` is never used. 

[[Singleton Design Pattern]]:
- Aim: ensure that there is only one instance of a given class at any point in the execution of the code.
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