Enums are like a class
```java
// declare
enum Size {
	SMALL, MEDIUM, LARGE, EXTRALARGE;
    // methods and fields	
}

// use
Size pizzaSize = Size.SMALL; 
```

Example:
```java
enum Size{
  SMALL, MEDIUM, LARGE, EXTRALARGE;

  public String getSize() {

    // this will refer to the initialized object
    switch(this) {
      case SMALL:
        return "small";

      case MEDIUM:
        return "medium";

      case LARGE:
        return "large";

      case EXTRALARGE:
        return "extra large";

      default:
        return null;
      }
   }
}

// use the method 
Size pizzaSize = Size.SMALL; 
System.out.println(pizzaSize.getSize());
```