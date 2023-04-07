### Motivation
- Say you have a few classes that implement an interface. These classes have some methods that are implemented exactly the same for the three of them, and some that vary. 
	- DUPLICATED CODE antipattern. 
- A better system would be to have a simplified parent abstract class that contains the implementations of the shared methods, which is then extended with the additional functionality. 
- The superclass is a *generalization* (white solid arrow) of the subclasses. 
- Java only supports single inheritance (1 superclass max); however since super classes can have super classes themselves, it is possible to get a tree-like hierarchy of classes. 
	- If a class does not declare to extend any class, by default it extends the library class Object. Class Object thus constitutes the root of any class hierarchy in Java code.

### Runtime vs. Static Types
- run-time type of an object = type of the actual object when it is initialized. Doesn't change throughout the life cycle of the object. 
- compile-time (static) type = type of the variable in which a reference to the object is stored at a particular point in the code. In a correct program the static type of an object can correspond to its run-time type, or to any supertype of its run-time type.
	- You can downcast to get from a supertype to the run-time type

### Overriding Fields
- `private` fields cannot be directly accessed by a subclass.
- The general principle in Java is that the fields of an object are initialized “top down”. This order is achieved simply by the fact that the ***first instruction of any constructor is to call a constructor of its superclass***, and so on.
	- If the superclass declares a constructor with no parameter, this call ***does not need to be explicit***.

### Overriding Methods
- Methods don't contain any information about the state of the object, and can be applied to subclasses by design. 
	- But we can also override methods in subclasses so that they have different behavior from in superclasses.
	- When you override a method, the subclass version is considered more specific, and used by default for all run-time instances of subclass. 
	- To call the super class version of the overridden method you must do `super.methodName();`. Super is like an instance variable (pointer). 
#### Annotation
- For a method to effectively override another one, it needs to have the same signature as the one it overrides
`@Override` is use to state that the following method is supposed to override another. if a method annotated with `@Override` does not actually override anything, a compilation error is raised.

### Overloading Methods
- Same method name, but different signature. 
- The selection procedure is to find all applicable methods and to select the most specific one.
- When you pass an object into a method, the *static type* is retained. The static type must match the method signature. 
- I recommend avoiding overloading methods except for widely used idioms (such as constructor overloading or library methods that support different primitive types). In many designs, the same properties can be obtained without overloading (i.e., by giving different names to the methods that take different types of arguments).

### Inheritance vs. [[Composition]]
Which option to choose will ultimately depend on the context. Composition-based reuse generally provides more run-time flexibility. This option should therefore be favored in design contexts that require many possible configurations, or the opportunity to change configurations at run-time. At the same time, composition-based solutions provide fewer options for detailed access to the internal state of a well-encapsulated object. In contrast, inheritance-based reuse solutions tend to be better in design contexts that require a lot of compile-time configuration, because a class hierarchy can easily be designed to provide privileged access to the internal structure of a class to subclasses (as opposed to aggregate and other client classes).

### Abstract Classes
- Technically, an abstract class represents a correct but incomplete set of class member declarations
	- The class can declare new abstract methods using the same `abstract` keyword, this time placed in front of a method signature.
- Note that because abstract classes cannot be instantiated, their constructor can only be called from within the constructors of subclasses. For this reason it makes sense to declare the constructors of abstract classes `protected`. 

### Final Methods and Classes
- Declaring a method to be `final` means that the method cannot be overridden by subclasses. The main purpose for declaring a method to be final is to clarify our intent, as designers, that a method is not meant to be overridden.
	- For the [[Template Method Design Pattern]], we want to ensure the template is respected in all subclasses, so it should be declared `final`. 
- The use of `final` with fields limits how we can assign values to variables, and does not involve inheritance, dynamic dispatch, or overriding.
- Classes declared to be `final` cannot be inherited
	- Good principle: "design for inheritance or else prohibit it"

### Proper Uses of Inheritance
- Inheritance is both a code reuse and an extensibility mechanism.
- To avoid major design flaws, inheritance should only be used for extending the behavior of a superclass.
	- it is bad design to use inheritance to restrict the behavior of the superclass, or to use inheritance when the subclass is not a proper subtype of the superclass.