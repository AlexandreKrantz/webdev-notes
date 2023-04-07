- One potential situation with inheritance is when some common algorithm applies to objects of a certain base type, but a part of the algorithm varies from subclass to subclass.
	- This means that part of the method is duplicated for each subclass implementation, and that makes the program more difficult to maintain and prone to errors/inconsistencies. 
	- The parts that are subclass-specific are left as `abstract` helper methods. 
- Solution: put all the common code in the superclass, and to define some “hooks” to allow subclasses to provide specialized functionality where needed. This idea is called the TEMPLATE METHOD design pattern. The name relates to the fact that the common method in the superclass is a “template”, that gets “instantiated” differently for each subclass. The “steps” are defined as non-private methods in the superclass.

### Example
```java
// The abstract class defines the template method and common steps
public abstract class Game {
   abstract void initialize();
   abstract void startPlay();
   abstract void endPlay();

   // The template method is final so subclasses cannot override it
   public final void play(){

      // Initialize the game
      initialize();

      // Start game
      startPlay();

      // End game
      endPlay();
   }
}

public class Football extends Game {

   @Override
   void endPlay() {
      System.out.println("Football Game Finished!");
   }

   @Override
   void initialize() {
      System.out.println("Football Game Initialized! Start playing.");
   }

   @Override
   void startPlay() {
      System.out.println("Football Game Started. Enjoy the game!");
  }

  // the play method is implemented properly in the super class and stays the same. 
}

// The main class uses the template method to play different games
```
