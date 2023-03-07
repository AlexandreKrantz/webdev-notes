A public **factory** class initializes and manages the objects privately. Client can only access certain number of objects using specified methods. 

- cleanly manage collections of low-level immutable objects.
- ensure the uniqueness of objects in a class. 
	- in other words, make it impossible to create two distinct instances that are equal. 
	- control the creation of flyweight objects through an access method. 

Step 1: make the constructor private
Step 2: make a new way for the client to create instances of the class. (`private static` method in class)


Flyweight pattern is used when we need to create a large number of similar objects. One important feature of flyweight objects is that they are **immutable**.

States that can have the same value for multiple objects are called *intrinsic*. States that are unique for every instance are *extrinsic*.

You also want to choose a datastructure to store your privately created instances. For example, you can have the user provide a key and store them in a HashMap (and then the user can retrieve the right object with the key).

### Example (ChatGPT)
```java
import java.awt.Color;
import java.util.HashMap;
import java.util.Map;

public class Circle {
    private final Color color;
    private final int radius;
    private static final Map<Color, Circle> circles = new HashMap<>();

    private Circle(Color color, int radius) {
        this.color = color;
        this.radius = radius;
    }

    public static Circle getCircle(Color color, int radius) {
        Circle circle = circles.get(color);
        if (circle == null) {
            circle = new Circle(color, radius);
            circles.put(color, circle);
        }
        return circle;
    }

    public void draw(int x, int y) {
        System.out.println("Drawing " + color + " circle with radius " + radius + " at (" + x + "," + y + ")");
    }
}
```

In this example, we have a `Circle` class that has two properties: a `Color` and a `radius`. We also have a `Map` of `Circle` objects that have been created, indexed by their color.

The `getCircle` method is a factory method that checks if a `Circle` object with the given color already exists in the map. If it does, it returns the existing `Circle` object. If not, it creates a new `Circle` object, adds it to the map, and returns it.

Finally, we have a `draw` method that takes an `x` and `y` coordinate and prints a message indicating that a circle of the given color and radius is being drawn at that coordinate.

By using the flyweight design pattern in this way, we can avoid creating multiple `Circle` objects with the same color, which can be useful if we have many circles of the same color in our application. Instead, we can reuse a single `Circle` object for each color, reducing the amount of memory required by our application.