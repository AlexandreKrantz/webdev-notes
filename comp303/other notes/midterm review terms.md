UML diagram notation (arrow types, etc)
- Aggregation: is part of (can be removed)
	- white diamond
- Composition: is entiredly composed of (removing voids the parent)
	- black diamond
- Generalization: eg. a parent class
	- white arrow solid
- Realization: implementation of an abstract class or interface
	- white arrow dotted
- Dependency: A depends on B. If you mess with B, then A will get messed up
	- dotted black arrow 
- Association: A has access to some B but B doesn't have access to A 
	- solid black arrow.
	- You can also have bidirection association (double arrow) where both A and B have some access to each other. 

m2
- encapsulation information hiding principle 
	- = only show the essential necessary information. 
- primitive obsession antipattern 
	- = don't use primitives like ints, boolean, etc unless they accurately represent the domain concept. Use enums instead.
- concept encapsulation 
	- = A class should encapsulate one concept. An interface should encapsulate an atomic concept whereas a class can encapsulate a higher level concept and implement many interfaces. 
- escaping references; when is it okay, when is it not
	- you can return references to immutable objects, but otherwise it's best to return a copy. 
- immutability of objects
	- An immutable object can only be mutated through methods and not directly?
- enums; methods inside enums
```java
enum Animals {
	DOG, CAT;
	public static Animals getCutest() {
		return DOG;
	}
}
```

- java generic classes + restricting generics
	- classes which have generic objects as constructor arguments
	- or specific sub classes
```java
class Smth<T extends bla > {
	Smth<T extends bla> (T a) {
	
	}
}
```

m3
- Iterator Design Pattern: benefits + how to use Iterator interface
	- Lazy evaluation, standard interface for interating over different types of collections,
	- the class you want to interate over implements `Iterable`. And then you make a `classNameInterator` class that implements `Iterator`. 
- separation of concerns (w/ respect to interface design).
	- each interface encapsulates an self-contained concept.
- Strategy design pattern
	- Have different versions for an algorithm. Each version is encapsulated by a different class (they all implement the same interface and have the same method name). 
	- That way depending on the class you can instantiate a different algorithm object. 
```java
interface flyingAbility {
	tryToFly()
}

class Flys implements flyingAbility {
	tryToFly() {
		// flying!	
	} 
}

class DoesntFly implements flyingAbility {
	tryToFly() {
		// cant fly :(
	}
}

abstract class Animal {
	flyingAbility

	fly () {
		flyingAbility.tryToFly()	
	}
}

class Dog extends Animal {
	flyingAbility

	fly () {
		flyingAbility.tryToFly()	
	}

	Dog() {
		flyingAbility = new Flys();	
	}
}

```

m4
- flyweight design pattern
	- aim is to have no two identical instances. make the constructor private and use a `get` method to get instances of the class. The get method checks if any indentical instances exist and either returns exist one or creates new one. 
```java
HashMap<K,V> map = new HashMap<K,V>();
```

- nested classes: inner classes, static inner classes, anonymous classes
	- inner class is instantiated with the outer one and has access to the outer one 
	- static inner class doesn't need to be instantiated
	- anonymous classes are an all-in-one implementation and initialization of an abstract class or interface
```java

InterfaceName bla = new InterfaceName() {
	// implementation 
}
```
- null object design pattern
	- have a special implementation of the interface that is for a null version of the class, that way you never need to use actual `null`. just have an `isNull` method in the abstract class set to return `false` by default. 
- object state 1
	- concrete state vs abstract state  
	- state space, 
	- stateless objects, 
	- useless states and speculative generality 
	- temporary field antipattern 
	- **nullability**
		- input validation vs design by contract 
	- using Java's optional types syntax (creating full vs empty, checking, getting)
```java
Optional smth;

smth = Optional.of("asldfj");
smth2 = Optoinal.empty();

if (smth.isPresent()) {
	return smth.get();
}
```
	- comparing objects: class, id, values 
	- minimizing states (with the final keyword)
- object state 2
	- invariant 
	- types of documentation 
	- Defining exceptions + when to use exceptions vs assertions 
- Singleton Design Pattern

m5
- The `Class` class
- Reflection (using it to access private fields) 
- JUnit `assertEquals`.
- How should test methods be designed (not private or static, not returning a value). Create a testSuite class and instantiate it before calling the individual methods. 
- a common idiom is to have one test class per project class, where the test class collects all the tests that test methods or other usage scenarios that involve the class.
- Arrange, Act, Assert pattern for designing a test method
- should you test private methods?
- testing with stubs (what is a stub?)
- Whitebox vs Blackbox testing
- Structural vs Functional testing
- How can we define 
- Statement coverage vs. branch coverage vs. path coverage

```java

```