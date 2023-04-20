GUI application code can be split into two categories:
- Framework code: deals with the UI elements. 
	- UI elements are represented by the *component library*. 
	- Basic UI logic/connectors (eg. event listener) are represented by the *application skeleton*
- Application code: application logic
	- Component graph organizes a hierarchy of UI elements. 
	- Event handling code 

See [[GUI Development Intro]]

- Application must be started by launching the framework using a special library method. This triggers the application skeleton to start an event loop. 
Call the static method `launch` from the main method:
```java
public class LuckyNumber extends Application { 
	public static void main(String[] pArgs) { 
		launch(pArgs);  
	} 
	@Override 
	public void start(Stage pPrimaryStage) { 
		... 
	} 
}
```
- The `launch` method launches the GUI framework, instantiates the `LuckyNumber` class AND executes the `start` method.
- Typically we initialize the component graph in the `start` method.
- The framework will automatically map interaction with UI components to events; it's your job to handle those events. 
	- This is an implementation of the [[Observer Design Pattern]], where the model is the GUI component

### Example:
```java
package helloworld;
 
import javafx.application.Application;
import javafx.event.ActionEvent;
import javafx.event.EventHandler;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.layout.StackPane;
import javafx.stage.Stage;
 
public class HelloWorld extends Application {
    public static void main(String[] args) {
        launch(args);
    }
    
    @Override
    public void start(Stage primaryStage) {
        primaryStage.setTitle("Hello World!");
        Button btn = new Button();
        btn.setText("Say 'Hello World'");
        btn.setOnAction(new EventHandler<ActionEvent>() {
 
            @Override
            public void handle(ActionEvent event) {
                System.out.println("Hello World!");
            }
        });
        
        StackPane root = new StackPane();
        root.getChildren().add(btn);
        primaryStage.setScene(new Scene(root, 300, 250));
        primaryStage.show();
    }
}
```

