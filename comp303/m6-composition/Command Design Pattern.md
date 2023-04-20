# Command Design Pattern
Concept: objects serve as *manageable units of functionality*
- Commands are pieces of state-changing functionality. Commands accomplish something. 
- Let's define commands as objects that implement the `Command` interface. 
	- The interface includes an `execute()` method. 
	- Interface may also include other methods for managing the command, such as `undo()`, `getDescription()`, etc...
- **Access to command target:** Since commands typically modify the state of other objects, the command object needs to have access to these other objects. Either a reference can be stored in the command object, or pass in as a parameter to the `execute()` method. 
- **Data flow:** interface methods `execute`, `undo`, etc will have return type `void`. So the result of the commands needs to be visible/returned some other way. 
- **Encapsulation of target objects:** Sometimes the command will need access to parts of the target object that are private (for example to `undo`). 
	- A solution for this is to create the command object inside of target object, by making a `createCommand` factory method inside of the target class. 
	- In this design, commands to operate on a `Deck` instance are obtained directly from the `Deck` instance of interest.
- **Storing of data:** To undo a command, you may want to store some data of the previous state in the command object. Like a cache. 

## Command Example (ChatGPT)
The Command design pattern is a behavioral design pattern that encapsulates a request as an object, thereby allowing you to parameterize clients with different requests, queue or log requests, and support undoable operations.

Four main elements of the command pattern:
1.  The Command: This is an interface that defines the `execute()` method, which encapsulates the operation to be performed.
2.  The Concrete Command: This is a class that implements the Command interface and contains the implementation for the `execute()` method.
3.  The Invoker: This is a class that requests the command to be executed.
4.  The Receiver: This is a class that performs the actual work when the command is executed.

Example of how the command pattern might be implemented in Java:
```java
// The Command interface
public interface Command {
    void execute();
}

// The Concrete Command classes
public class LightOnCommand implements Command {
    private Light light;

    public LightOnCommand(Light light) {
        this.light = light;
    }

    public void execute() {
        light.turnOn();
    }
}

public class LightOffCommand implements Command {
    private Light light;

    public LightOffCommand(Light light) {
        this.light = light;
    }

    public void execute() {
        light.turnOff();
    }
}

// The Invoker class
public class Switch {
    private Command onCommand;
    private Command offCommand;

    public Switch(Command onCommand, Command offCommand) {
        this.onCommand = onCommand;
        this.offCommand = offCommand;
    }

    public void turnOn() {
        onCommand.execute();
    }

    public void turnOff() {
        offCommand.execute();
    }
}

// The Receiver class
public class Light {
    public void turnOn() {
        System.out.println("Light is on");
    }

    public void turnOff() {
        System.out.println("Light is off");
    }
}

// Client code
Light light = new Light();
Command lightOnCommand = new LightOnCommand(light);
Command lightOffCommand = new LightOffCommand(light);

Switch lightSwitch = new Switch(lightOnCommand, lightOffCommand);
lightSwitch.turnOn();
lightSwitch.turnOff();

```

### Explanation
In this example, the `Command` interface defines the `execute()` method. The `LightOnCommand` and `LightOffCommand` classes are concrete implementations of the `Command` interface. They contain a reference to the `Light` object, which is the receiver of the command.

The `Switch` class is the invoker (the class that requests the command to be executed). It contains references to both the `LightOnCommand` and `LightOffCommand` objects and calls their `execute()` methods when the switch is turned on or off.

The `Light` class is the receiver. It contains the actual implementation of the `turnOn()` and `turnOff()` methods, which are called by the `execute()` methods of the `LightOnCommand` and `LightOffCommand` classes.

The client code creates a `Light` object and two `Command` objects (`LightOnCommand` and `LightOffCommand`). It then creates a `Switch` object and passes in the two `Command` objects. Finally, the client code turns the switch on and off, which causes the corresponding `Command` objects to be executed.
