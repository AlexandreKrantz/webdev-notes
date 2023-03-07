### Types and Interfaces:
But how does object of different classes interact?
==interactions between objects are mediated through interfaces==

#### Decoupling Behaviour from Implementation:
An interface to  a class consists of the methods of that class that are accessible (or visible) to another class. -> it takes into account access modifiers and scoeps.
For now, we **define an interface to a class as the set of its public methods**.
```java
public class Client {
	private Deck aDeck = new Deck();
}
public class Deck {
	public void shuffle() {...}
	public Card draw() {...}
	public boolean isEmpty() {...}
}
```
The interface of class Deck consists of three methods.
Here we would say that the interface of class Deck is fused, or coupled, with the class definition. 
The interface of class Deck is just a consequence of how we defined class Deck.
For client code it needs to invoke method shuffle() of a field or local variable of type deck.

There are situations that we want to decouple the interface of a class from its implementation
Maybe we want to draw cards not from a standard deck of 52 cards.

```java
public static List<Card> drawCards(Deck pDeck, int pNumber) {
	List<Card> result = new ArrayList<>();

	for(int i=0; i < pNumber && !pDeck.isEmpty(); i++){
		reuslt.add(pDeck.draw());
	}
	return result;
}
```
This method can only be used with sequence of cards that are instances of class Deck. But the same code could be useful for any object that has the two required methods draw() isEmpty().
It would be useful to specify an abstraction of an interface without tying it to a specific class.
	This is where Java interface types come in. Java interface types provide a specification of the methods that it should be possible to invoke on an object. With interface types, we can define an abstraction CardSource as any object that supports a draw() and isEmpty() method.
```java
public interface CardSource {
	/**
	 *Returns a card from the source.
	 *
	 * @return the next available card.
	 * @pre !isEmpty()
	 */
	 Card draw();

	/**
	 * @return True if there is no card in the source.
	 */
	 boolean isEmpty();
}
```
This interface declaration lists two methods, and includes comments that specify the behaviour of each method. The methods have specification and no implementation.
Interface method declaration are a specification and not an implementation, details of what the methods are expecterd to perform are very important.
In Java terminology, methods that do not have an implementation are called ==abstract methods.==
To tie a class with an interface use the implement keyword
```java
public class Deck implements CardSource P {
	...
}
```
The implements keyword has two related effects:
- A formal guarantee that instances of the class type will have concrete implementation for all the methods in the interface type. This guarantee is enforeced by the compiler.
- It creates a subtype relationship between the implementing class and the interface type. Deck is a type of CardSource.
The subtyoe relation between a concrete class and an interface type -> polymorphism (the ability to have different shapes)
Here CardSource is an abstraction that can present itself in different concrete shapes, which are different implementations of the CardSource interface.
It is possible to assign a value to a variable if the value is of the same type or a subtype of the type of the variable.
```java
CardSource source = new Deck();
```
We can make our implementation of the drawCards method much more usable:
```java
public static List<Card> drawCards(CardSource pSource, int pNumber) {
	List<Card> result = new ArrayList<>();
	for (i=0; i < pNumber && !pSource.isEmpty(); i++){
		result.add(pSource.draw());
	}
	return result;
}
```
This method is now applicable to objects of any class that implements the CardSource interface.
Another illustration of the use of polymorphism is the use of concrete vs abstract types in the Java Collections FrameWork:
```java
List<String> list = new ArrayList<>();
```
List is an interface that specifies the usual services (add, remove, etc)
ArryaList is an implementation of this servies that uses an array
WE could replace ArrayList by LinkedList and the code will compile -> because even though their implementations are different but both provide the methods required by the List interface.
##### The Pros of Polymorphism:
- ==Loose coupling, because the code using a set of methods is not tied to a specifc implementation of these methods==
- ==Extensibility, because we can easily add new implementaitons of an interface (new shapes in the polymorphic relation)==

#### Specifying Behavior with Interface Types:
do i need a separate interface? and what should this interfcae specify?
One good illustration of both the purpose and usefulness of interfaces in Java is the Comparable interface.
Shuffling a deck of cards:
```java
public class Deck {
	private List<Card> aCards = new ArrayList<>();

	public void shuffle() {
		Collections.shuffle(aCards);
	}
}
```
As its name implies, the library method shuffle randomly reorders the objects in the argument collection. This is an example of code euse because it is possible to reuste the library method to reorder any kind of collection. Here reuse is easy because to shuffle a collection we do not need to know anything about the items being shuffled.
What if we want to ruse code to sort the cards in the deck? 
Java collections class conveniently supplies us with a number of sorting functions
```java
List<Card> cars = ...;
Collections.sort(cards);
```
This leads to a cryptic compilation error. Different orderings are possible (by rank, then suit, ...)
The `Comparable<T>` interface helps solve this problem by defining a piece of behavior related specifically to the comparison of objects, in the form of single compareTo(T) abstract method.
The specification for this method is that it should return 0 if the implicit argument is equal to the explicit argument , a negative integer if it should come before, and a positive integer if it should come after.
The internal implementation of Collections.sort can rely on it to compare the objects it shoud sort.
So, from the point of view of sort, it really does not matter what the object is , as long it is comparable with another object.
To make it possible for us to sort a list of cards, we therefore have to provide this comparable behavior and declare it with the implements keyword:
```java
public class Card implements Comparable<Card> {
	public int compareTo(Card pCard) {
		return aRank.ordinal() - pCard.aRank.ordinal();
	}
}
```
Java enumerated types inmplement Comparable by comparing instances of an enumerated type according to their ordinal value.
```java
public class Card implements Comparable<Card> {
	public int compareTo(Card pCard){
		return aRank.compareTo(pCard.aRank);
	}
}
```
Using small interfaces encourages the respect of a software design principle called seperation of concerns.
The idea of seperation of concerns is that one abstraction should map to a single concern (or area of interest).

#### Class Diagrams:
Class diagrams represent a static , or compile time, view of a software system.
They are useful to represent how types are defined and related, but poor for capturing any kind of run-time property of the code.
Type of UML diagrams that are closest to the code.
Aggregation, association and dependency -> two classes are somehow related
Navigability (represented with an arrow head), models how code supports going from objects of one type to objects of another type, it can be unidirectional, bidirectional, or unspecified.

![[photo2.png]]
interface: "A coherent set of public features and obligations"
Generalization : "A taxonomic relationship between a more general element and a more specific element"   
Realization: "The relationship between a spcification and its implementation"
Association: "The semantic relationship between two or more classifiers that involves connections among their instances"
Dependency : " A relationship between two elements in which a change to one element may affect or supply information needed by the other element"
Class: "The descriptor for a set of objects that share the same attributes, operations"


A class diagram that models some of the key relations between the design elements for a card game.

![[photo3.png]]

- The box that represents class Card does not have attribtues for aRank and aSuit, because they are represented as aggregations to Rank and Suit enumerated types. (it would be a modeling error to have both an attrubute and an aggregation to represent a given field)
- The methods of class Card are not represented because they are just the constructor and the accessors.
- Collections depend on Comparable
- To indicate that  a method is static in jetUML, we can prefix the member's name with the keyword static.
- The model includes ==cardinalities== to indicate, for example, that a deck instance will aggregate between zero and 52 instances of Card. (typical values for an association's cardinality include a specific number (for example, 1), the wildcard * (which means zero or more), and ranges as M..N, which means between M and N inclusive)
#### Function Objects & Switch Statements:
An interface type often defines only a subset of operations of classes that implement it.
There are situations, where it is convinient to define classes that specialize in implementing only the behavior required by a small interface with only one method.
What if we have a game that switches strategies to sort cards?
One could tweak the code of compareTo, by relying on a global variable that stores the required strategy and switiching the comparison strategy based on this flag.
However such a flag variable would degrade the separation of concerns between representing a card and knowledge of how the card should be sorted, and generally make the code harder to understand.
Using this kind of switching is considered a design antipattern called SWITCH STATEMENT
A more promising solution is to move the comparison code to a seperate object. This solution is supported by the `Comparator<T>` interface. The abstract method in this interface is compare.
```java
int compare(T pObject1, T pObject2)
```
This method takes two arguments instead of one, comparead to Comparable. Instead of comparing the implicit parameter (this) to the explicit parameter, this compares two explicit parameters with each other.
```java
Collections.sort(List<T> list, Comparator<? super T> c)
```
This method can sort a list of objects that do not necessarily implement the Comparable interface, by taking as argument an object guarantedd to be able to compare two instances of the items in the list. One can now define a rank firt comparator:
```java
public class RankFirstComparator implements Comparator<Card> {
	public int compare(Card pCard1, Card pCard2) {
	/*comparison code*/
	}
}
```
we could do the same with suit
and then sort with desired comparator:
```java
Collections.sort(aCards, new RankFirstComparator());
```
In this scenario, an instance of Comparator is simply an object that provides the implementation for a single method. Such objects are referred to as ==function objects==.
If the comparator classes need access to the private members of the classes they compare , we can declare the comparator classes as nested classes.
```java
public class Card{
	static class CompareBySuitFirst implements Comparator<Card> {
		public int compare(Card pCard1, Card pCard2){
			/* Comparison code */
		}
	}
}
```
The client code would look like this:
```java
Collections.sort(aCards, new Card.CompareBySuitFirst());
```
Another option is to define compartor classes as anonymous classes.
In cases where the comparator is refered only once this makes a lot of sense
```java
public class Deck {
	public void sort() {
		Collections.sort(aCards, new Comparator<Card> {
			public int compare(Card pCard1, Card pCard2) {
				/*Comparison code*/
			}
		})
	}
}
```
