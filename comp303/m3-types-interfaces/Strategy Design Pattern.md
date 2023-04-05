- Use composition, rather than inheritance. Inheritance is not intended for code reuse. 
- Defines a family of algorithms, encapsulates each one, and makes them interchangable. 
	- Decouple the algorithm from the client (using the algorithms). The algorithms can vary independent of the client behavior. 
- For example, different sorting algorithms used by a list. 
[Strategy Pattern – Design Patterns (ep 1) - YouTube](https://www.youtube.com/watch?v=v9ejT8FO-7I)

You don't want to have a direct inheritance structure, because there may be cases where subclasses will share some methods, and differ in other areas. Instead, create interfaces for the various algorithms that will vary. 


Decoupling: encapsulate the behavior that varies (flying). 
```java
// The interface is implemented by many other
// subclasses that allow for many types of flying
// without effecting Animal, or Flys.

// Classes that implement new Flys interface
// subclasses can allow other classes to use
// that code eliminating code duplication

// I'm decoupling : encapsulating the concept that varies

public interface Flys {

   String fly();
   
}

// Class used if the Animal can fly

class ItFlys implements Flys{

	public String fly() {
		
		return "Flying High";
		
	}
	
}

//Class used if the Animal can't fly

class CantFly implements Flys{

	public String fly() {
		
		return "I can't fly";
		
	}
	
}


class Animal {
	public Flys flyingType; // variable that can be instantiated to ones of the Flys implementations (either ItFlys or CantFly)

	public String tryToFly() {
		return flyingType.fly(); 
	}

	public void setFlyingAbility(Flys newFlyType) {
		flyingType = newFlyType; 
	}
}

class Dog extends Animal { 
	Dog() {
		super();
		setSound("Bark");
		flyingType = new CantFly() 
	}
}

class Bird extends Animal { 
	Dog() {
		super();
		setSound("Bark");
		flyingType = new ItFlys() 
	}
}


public class AnimalPlay{
	
	public static void main(String[] args){
		
		Animal sparky = new Dog();
		Animal tweety = new Bird();
		
		System.out.println("Dog: " + sparky.tryToFly());
		
		System.out.println("Bird: " + tweety.tryToFly());
		
		// This allows dynamic changes for flyingType
		
		sparky.setFlyingAbility(new ItFlys());
		
		System.out.println("Dog: " + sparky.tryToFly());
		
	}
	
}
```


![[Pasted image 20230120120050.png]]

![[Pasted image 20230120120145.png]]

