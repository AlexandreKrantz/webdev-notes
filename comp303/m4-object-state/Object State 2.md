Review [[Design by Contract]]
- An invariant is **a condition or relation that is always true**. Specify them with `@invariant`

- Assert statements throw `AssertionError`
Syntax
```java
assert Expression; // if Expression is false, it throws an AssertionError
```

Types of documentation:
- Interface: comment block before class (or method) declaration, explaining what the class is about and how to use it. 
- Data fields: comment next to variable declaration, explaining its use.
- Implementation comments: comment inside a method, perhaps explaining an algorithm or operation. 

## Design for Robustness
How to check preconditions in practice (convention): 
- public method: Explicit checks that throw particular, specified exceptions
- private or protected method: Use assertion to test a nonpublic method's precondition that you believe will be true no matter what a (external) client does with the class

Example: 
```java
/**
* … …
* @param Student to be enrolled to the Course
* @throws IllegalStateException if isFull()
*/
public void enroll(Student pStudent) {
	if (pStudent == null) {
		throw new NullPointerException(); 
	} if (isFull()) {
		throw new IllegalStateException(); 
	}
	aEnrollment.add(pStudent);
}

```
	- initialize an exception and throw it. 
	- these are examples of runtime exceptions.

Runtime exception:
- indicates a precondition (contract) violation. The client is responsible to do better. 
- interrupts the execution of the program. 

Checked exceptions:
- indicated in method signature. Eg. `public void enroll(Student pStudent) throws CourseFullException`
	- Define the exception: `CourseFullException extends Exception`. 
```java
class myCustomException extends Exception { 
	myCustomException() {
		super();
	}

	// optional
	myCustomException(String msg) {
		super(msg);
	}
}
```
- Client is responsible for handling it (method should be called in a try/catch). 

## Objects of [[Nested Classes]]
