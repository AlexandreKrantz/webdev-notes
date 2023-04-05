## The `Class` class
- Basically, when you call `getClass` on an object (like a string for example), you are returned a `Class` object. 
- It provides a way to retrieve metadata about a class, such as its name, fields, methods, constructors, interfaces, annotations, and more.

- `Class` has no public constructor. Instead a Class object is constructed automatically by the Java Virtual Machine. It is a final class, so we cannot extend it. 
- The primitive Java types (boolean, byte, char, short, int, long, float, and double), and the keyword _void_ are also represented as Class objects by default.

### Methods
- When you initialize objects such as strings, they are automatically attached to a class object. In this case, a `Class<String>` object. The object has methods such as `getName`. 
```java
 void printClassName(Object obj) {
	 System.out.println("The class of " + obj + " is " + obj.getClass().getName());
 }

printClassName("hello"); // prints "String"
```

Other methods of the `Class` object include: 
- the `getDeclaredFields()` method to obtain an array of all the fields declared in the class,
- the `getDeclaredMethods()` method to obtain an array of all the methods declared in the class.

### Reflection
the Class class can be used for reflection, which is a powerful mechanism in Java that allows you to examine and modify the behavior of a class at runtime.

Using reflection, you can access private fields and even intialize objects without calling the constructor. Useful for [[Unit Testing#Testing Private Methods |testing private methods]]!

#### Example
Suppose we have a class `Person` defined as follows:
```java
public class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public void sayHello() {
        System.out.println("Hello, my name is " + name + " and I am " + age + " years old.");
    }
}
```

Now let's say we want to create an instance of the `Person` class and invoke the `sayHello()` method using reflection. Here's how we can do it:
```java
import java.lang.reflect.*;

public class ReflectionExample {
    public static void main(String[] args) throws Exception {
        // Get the Class object for the Person class
        Class<?> personClass = Class.forName("Person");

        // Create a new instance of the Person class using the no-argument constructor
        Object person = personClass.getDeclaredConstructor().newInstance();

        // Set the value of the "name" field using reflection
        Field nameField = personClass.getDeclaredField("name");
        nameField.setAccessible(true);
        nameField.set(person, "Alice");

        // Set the value of the "age" field using reflection
        Field ageField = personClass.getDeclaredField("age");
        ageField.setAccessible(true);
        ageField.setInt(person, 30);

        // Invoke the "sayHello" method using reflection
        Method sayHelloMethod = personClass.getDeclaredMethod("sayHello");
        sayHelloMethod.setAccessible(true);
        sayHelloMethod.invoke(person);
    }
}
```

In this example, we use the `Class.forName()` method to obtain the `Class` object for the `Person` class. Then, we create a new instance of the `Person` class using the `newInstance()` method, which calls the no-argument constructor.

Next, we use reflection to set the values of the `name` and `age` fields of the `Person` object, and then we invoke the `sayHello()` method using reflection.

Note that we need to call `setAccessible(true)` on the `nameField`, `ageField`, and `sayHelloMethod` objects to override their access restrictions and allow us to modify or invoke them using reflection. In practice, you should use reflection with caution and only when necessary, as it can make your code more complex and less readable.