Nested classes can be divided into two broad categories: static nested classes and inner classes. In turn, there are two special categories of inner classes: anonymous classes and local classes

### Inner Classes
Instances of an inner class automatically get a reference to the corresponding instance of their enclosing class (called the outer instance).
Within an inner class, you can access the current instance of the outer class with the following syntax: `enclosingClassName.this`

#### Static Inner Classes
Java also allows the declaration of `static` nested classes. The main difference between static nested classes and inner classes is that static nested classes are not linked to an outer instance. As such, they are mostly used for encapsulation and code organization. 

### Anonymous Classes
1.  Anonymous inner classes are defined in the same statement as their instantiation.
2.  Anonymous inner classes can only be created as instances of either an interface or an abstract class.
3.  Anonymous inner classes have access to the members of the enclosing class.
4.  Anonymous inner classes can override methods of the interface or abstract class they are implementing.
5.  Anonymous inner classes cannot have any constructor because they have no name and cannot be named.

```java
abstract class Shape {
  abstract void draw();
}

Shape shape = new Shape() {
  @Override
  void draw() {
    System.out.println("Drawing a shape");
  }
};

shape.draw();

```

In this example, an anonymous inner class is created that extends the abstract class `Shape`. The `draw` method of the abstract class is overridden in the anonymous inner class, and an instance of the anonymous inner class is created and assigned to the `shape` variable.

