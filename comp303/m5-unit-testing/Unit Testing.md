A unit is a small, self-contained functionality. 

JUnit is a unit testing framework: [JUnit 5](https://junit.org/junit5/)

In practice:
```java
import org.junit.jupiter.api.Test; 
import static org.junit.jupiter.api.Assertions.*;

// now we can use this method to check if methods give the desired result
assertEquals("Lily", s.getFirstName());
```

Specification: 
```java
public static void assertEquals(Object expected, Object actual)
```
	- Asserts that expected and actual are equal.
	- If both are null, they are considered equal

Annotation `@Test` is a form of metadata. Helps the compiler apparently? 
Put above the test methods to signal that they are, in fact, test methods. 
- Test methods must *not* be private or static. They also should not return a value. 

- Each unit test is its own method
- A collection of tests for a project is known as a test suite.


## AAA (Arrange, Act, Assert) Pattern
1.  Arrange: This step is used to set up the test preconditions and arrange the objects, data, and environment necessary for the test to run.
2.  Act: This step is used to execute the code that is being tested. This is where the test actually interacts with the code and verifies its behavior.
3.  Assert: This step is used to verify that the code under test produced the expected results. This is done by making assertions about the state of the system or the output of the code being tested.

```java
import org.junit.Test;
import static org.junit.Assert.assertEquals;

public class CalculatorTest {
  @Test
  public void testAddition() {
    // Arrange
    Calculator calculator = new Calculator();
    int expected = 4;
    
    // Act
    int result = calculator.add(2, 2);
    
    // Assert
    assertEquals(expected, result);
  }
}
```

## Encapsulation and Unit Testing
A design issue that often comes up when testing is that to write the test we want, we need some functionality that is not part of the interface of the class being tested. 

It's better to provide this functionality in the form of a helper method in the testing class. Rather than add extra complexity to the interface of the production class. 

#### Testing Private Methods
- Private methods are internal elements of other, accessible methods, and therefore are not really “units” that should be tested.
	- They should especially not be tested if they are 
	- Code in private methods is tested indirectly through the execution of the accessible methods that call them
- Sometimes, it can still be useful to test them. Especially if they are performing an isolated role. In this case we need to bypass the method's access restriction. 
	- In this case, you create a public helper method in the class itself that tests the private method. 
Another technique for accessing private methods/fields during runtime is [[Metaprogramming#Reflection|reflection]].

## Testing with Stubs
- In some cases, it's hard to test small units of code in isolation. Sometimes methods have dependencies on other methods or the state of the environment.
	- A stub is a simplified version of an object created for testing purposes that reproduces the minimum requirements to test the method. 
A stub is a piece of code used in unit testing to simulate the behavior of a component or module that the unit being tested depends on. It is typically used to isolate the unit being tested, so that it can be tested in isolation, without the need to test or use the entire system or its dependencies.

A stub typically implements just enough functionality to allow the unit being tested to run and produce a result, without actually performing the full functionality of the component or module it is replacing. For example, if a unit being tested depends on a database, a stub could be used to simulate the database by returning predefined values for certain queries, without actually connecting to a real database.

Stubs can be useful for several reasons in unit testing:
-   Isolation: They allow the unit being tested to be isolated from the rest of the system, which can make testing faster, more reliable, and more repeatable.  
-   Control: They allow the test to control the input and output of the unit being tested, which can make it easier to test specific scenarios or edge cases.
-   Avoiding side-effects: They can prevent the unit being tested from affecting other parts of the system, such as a real database, file system, or network, which can make testing safer and more predictable.
-   Simplifying tests: They can make the tests simpler and easier to write and maintain, by allowing the test writer to focus on the unit being tested, rather than on the complexity of the system it depends on.

## Test Coverage
Usually it's not possible to test the entire input space. Efficient testing = minimal test cases for maximal testing coverage. 
- Black box testing = cover as much of the specified behavior of the UUT (unit under test). 
- White box testing = cover as much of the implemented behavior of the UUT as possible. 

- Structural testing involves examining the internal structure of the software, typically at the code level, to evaluate its behavior and ensure that it meets specific requirements. 
- Functional testing focuses on testing the external behavior of the software by verifying that it performs as expected and meets the functional requirements. This type of testing is typically performed at the user interface level and can involve testing features such as input validation, output correctness, and error handling.

#### Statement coverage 
= number of statements exectued, divided by total number of statements. 
- a statement refers to a single executable line of code in a program. It is the smallest unit of execution in a program that can be evaluated for coverage. A statement can include any type of executable code, such as assignments, conditional statements, loops, function calls, or variable declarations.

#### Branch coverage 
= number of program branches (aka decision points), divided by total number of branches. 
Example:
```python
def divide(a, b):
    if b == 0:
        return None
    else:
        return a / b
```

To achieve statement coverage, a test case could simply call the `divide()` function with any two values, which would execute both the `if` and `else` blocks. However, this test case does not test the scenario where `b` is equal to zero, which is an important boundary case that needs to be tested.

To achieve branch coverage, a test case would need to include both a scenario where `b` is not equal to zero, as well as a scenario where `b` is equal to zero. This would require the test case to execute both the `if` and `else` blocks of the `divide()` function.

If only statement coverage was used, it's possible that the defect where `b` is zero would not be detected since it was not tested. However, by using branch coverage, the scenario where `b` is zero is specifically tested, which ensures that all possible paths through the code are evaluated.


#### Path coverage
- A path is a sequence of statements and branches that begins with the first executable statement of a program and ends with the last statement.
- To achieve path coverage, every possible path through a program must be tested at least once. This includes not only the "normal" execution paths, but also any alternative or exceptional paths that may be taken under certain conditions.
Example of path coverage being better than branch coverage: 
```python
def calculate(a, b, c):
    if a == 0:
        return None
    else:
        x = b * b - 4 * a * c
        if x < 0:
            return None
        else:
            return (-b + math.sqrt(x)) / (2 * a), (-b - math.sqrt(x)) / (2 * a)
```
To achieve branch coverage, test cases would need to be created to test the two possible outcomes of the if statement. However, if we only rely on branch coverage, it is possible that we would miss the scenario where the value of `x` is exactly zero, which would cause a division by zero error when calculating the return values.

