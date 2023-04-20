# Decorator Design Pattern
The Decorator design pattern is a structural design pattern that *allows behavior to be added* to an individual object, either statically or dynamically (at runtime), *without affecting the behavior of other objects* from the same class. 

- Decorator class wraps the original object and provides additional functionality while maintaining the same interface as the original object.
- When implementing the DECORATOR design pattern in Java, it is a good idea to specify as `final` the field that stores a reference to the decorated object, and to initialize it in the constructor.
- **Decorated objects lose their identity**. In other words, because a decorator is itself an object that wraps another object, a decorated object is not the same as the undecorated object.

## Example
```java
public interface Coffee {
    double getCost();
    String getDescription();
}

public class BasicCoffee implements Coffee {
    public double getCost() {
        return 1.00;
    }

    public String getDescription() {
        return "Basic Coffee";
    }
}

public abstract class CoffeeDecorator implements Coffee {
    private final Coffee decoratedCoffee;

    public CoffeeDecorator(Coffee decoratedCoffee) {
        this.decoratedCoffee = decoratedCoffee;
    }

    public double getCost() {
        return decoratedCoffee.getCost();
    }

    public String getDescription() {
        return decoratedCoffee.getDescription();
    }
}

public class Milk extends CoffeeDecorator {
    public Milk(Coffee decoratedCoffee) {
        super(decoratedCoffee);
    }

    public double getCost() {
        return super.getCost() + 0.50;
    }

    public String getDescription() {
        return super.getDescription() + ", Milk";
    }
}

public class Sugar extends CoffeeDecorator {
    public Sugar(Coffee decoratedCoffee) {
        super(decoratedCoffee);
    }

    public double getCost() {
        return super.getCost() + 0.25;
    }

    public String getDescription() {
        return super.getDescription() + ", Sugar";
    }
}
```

In this example, the `Coffee` interface defines the basic behavior of a Coffee object. The `BasicCoffee` class is a concrete implementation of the `Coffee` interface that represents a basic coffee.

The `CoffeeDecorator` abstract class is a decorator class that implements the `Coffee` interface and wraps a `Coffee` object. The `Milk` and `Sugar` classes are concrete decorators that add new behavior to the `Coffee` object by implementing the `getCost()` and `getDescription()` methods.

The `Milk` decorator adds the cost of milk to the base cost of the coffee and appends ", Milk" to the description. The `Sugar` decorator adds the cost of sugar to the base cost of the coffee and appends ", Sugar" to the description.

By using the Decorator pattern, we can create a `BasicCoffee` object and then wrap it with `Milk` and `Sugar` decorators to create a customized coffee object with additional behavior.

### Using the Example
```java
Coffee myCoffee = new Sugar(new Milk(new BasicCoffee()));

System.out.println("Cost: " + myCoffee.getCost() + ", Description: " + myCoffee.getDescription());
```


## Combining Composite and Decorator
Refresh yourself: [[Composite Design Pattern]]

In this combined approach, the Composite pattern is used to create a hierarchical tree structure of objects, where a component can either be an individual object or a group of objects. The Decorator pattern is then used to add additional behavior to each object in the tree structure, without affecting the behavior of other objects in the same tree.

### Example

```java
public interface Graphic {
    void draw();
}

public class Circle implements Graphic {
    public void draw() {
        // draw circle
    }
}

public class Rectangle implements Graphic {
    public void draw() {
        // draw rectangle
    }
}

public class Group implements Graphic {
    private List<Graphic> graphics = new ArrayList<>();

    public void add(Graphic graphic) {
        graphics.add(graphic);
    }

    public void remove(Graphic graphic) {
        graphics.remove(graphic);
    }

    public void draw() {
        for (Graphic graphic : graphics) {
            graphic.draw();
        }
    }
}

public abstract class GraphicDecorator implements Graphic {
    private Graphic decoratedGraphic;

    public GraphicDecorator(Graphic decoratedGraphic) {
        this.decoratedGraphic = decoratedGraphic;
    }

    public void draw() {
        decoratedGraphic.draw();
    }
}

public class ShadowDecorator extends GraphicDecorator {
    public ShadowDecorator(Graphic decoratedGraphic) {
        super(decoratedGraphic);
    }

    public void draw() {
        drawShadow();
        super.draw();
    }

    private void drawShadow() {
        // draw shadow
    }
}

public class BorderDecorator extends GraphicDecorator {
    public BorderDecorator(Graphic decoratedGraphic) {
        super(decoratedGraphic);
    }

    public void draw() {
        drawBorder();
        super.draw();
    }

    private void drawBorder() {
        // draw border
    }
}

```

**Composite design pattern:** In this example, the `Graphic` interface defines the basic behavior of a graphic object, which can either be a `Circle`, a `Rectangle`, or a `Group` of graphics. The `Group` class represents a composite object that can contain other graphics as its children.

**Decorator design pattern:** The `GraphicDecorator` abstract class is a decorator class that implements the `Graphic` interface and wraps a `Graphic` object. The `ShadowDecorator` and `BorderDecorator` classes are concrete decorators that add new behavior to the `Graphic` object by implementing the `draw()` method.

#### Using the Example

```java
Group myGroup = new Group();
myGroup.add(new Circle());
myGroup.add(new Rectangle());
myGroup.add(new ShadowDecorator(new BorderDecorator(new Circle())));
myGroup.draw();
```