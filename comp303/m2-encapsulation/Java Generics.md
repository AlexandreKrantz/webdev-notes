classes like HashSet, ArrayList, HashMap, etc., use generics very well

Generics only work with reference types

**Conventions**
The type parameters naming conventions are important to learn generics thoroughly. The common type parameters are as follows:
-   T – Type
-   E – Element
-   K – Key
-   N – Number
-   V – Value

### Generic Classes
```java
// To create an instance of generic class 
BaseType <Type> obj = new BaseType <Type>()

// For example
// We use < > to specify Parameter type
class Test<T> {
    // An object of type T is declared
    T obj;
    Test(T obj) { this.obj = obj; } // constructor
    public T getObject() { return this.obj; }
}

// Driver class to test above
class Main {
    public static void main(String[] args)
    {
        // instance of Integer type
        Test<Integer> iObj = new Test<Integer>(15);
        System.out.println(iObj.getObject());
  
        // instance of String type
        Test<String> sObj
            = new Test<String>("GeeksForGeeks");
        System.out.println(sObj.getObject());
    }
}
```


Example of multiple generic types in a class:
```java
class Test<T, U>
{
    T obj1;  // An object of type T
    U obj2;  // An object of type U
  
    // constructor
    Test(T obj1, U obj2)
    {
        this.obj1 = obj1;
        this.obj2 = obj2;
    }
  
    // To print objects of T and U
    public void print()
    {
        System.out.println(obj1);
        System.out.println(obj2);
    }
}
```

### Generic Methods/Functions
```java
// Java program to show working of user defined
// Generic functions
  
class Test {
    // A Generic method example
    static <T> void genericDisplay(T element)
    {
        System.out.println(element.getClass().getName()
                           + " = " + element);
    }
  
    // Driver method
    public static void main(String[] args)
    {
        // Calling generic method with Integer argument
        genericDisplay(11);
  
        // Calling generic method with String argument
        genericDisplay("GeeksForGeeks");
  
        // Calling generic method with double argument
        genericDisplay(1.0);
    }
}
```

### Adding Restrictions
Generic type can only be Deck or a subtype of Deck:
```java
public class Pair<T extends Deck> {
	//...
	public boolean isTopCardSame() { // method using Deck methods
		Card topCardInFirst = aFirst.draw(); 
		Card topCardInSecond = aSecond.draw(); 
		return topCardInFirst.equals(topCardInSecond); 
	}
}
```
--> This enables us to use the methods of Deck directly in the Pair class. 


