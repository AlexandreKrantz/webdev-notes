# OOP Principles
- These principles are help guidelines, not rules.

## Single Responsibility Principles
- A class (or object, or module, etc) should only have _one_ responsibility (one reason to change). 
- In other words, an object’s definition should only have to be modified due to changes to a single responsibility within the system.
- Object Role Stereotypes are a set of general, pre-established roles which commonly occur across object-oriented architectures. 
  - Information holder = an object designed to know certain information and provide that information to other objects.
  - Structurer = an object that maintains relationships between objects and information about those relationships.
  - Controller =  an object designed to make decisions and control a complex task.
  - Coordinator = an object that doesn’t make many decisions but, in a rote or mechanical way, delegates work to other objects.
  - Interfacer = an object that transforms information or requests between distinct parts of a system.
- If you find yourself wanting to call a function `loginUserAndGetGroups()`, you’re probably breaking the Single Responsibility Principle. Break these functions into two separate ones.
- For every function you create, think if there’s a useful part which can be extracted into an even smaller function.
## Other SOLID Principles

    S – Single Responsibility Principle
    O – Open-Closed Principle
    L – Liskov Substitution Principle
    I – Interface Segregation Principle
    D – Dependency Inversion Principle

### Open-Closed Principle
- Modules should be open to extension, but closed to modification. You shouldn't need to hard code modifications, but instead there should be set/add methods for the properties you want to change. 

### Liskov Substitution Principle
- Every subclass/derived class should be substitutable for their base/parent class.
  - A subclass should override the parent class methods in a way that does not break functionality from a client’s point of view.
- Sometimes abstractions that sound right in natural language don't work in OOP. If there is some additional constraint/requirement, then the abstraction doesn't work. Eg: a square class should extend from rectangle because it has a constraint on the width and height being the same.

### Interface Segregation
- Whenever you expose a module for outside use, make sure only the bare essential parameters/setup are required to use it and the rest are optional.
- A client should never be forced to implement an interface that it doesn’t use or clients shouldn’t be forced to depend on methods they do not use.

### Dependency Inversion Principle
- high level modules should not depend on low level modules; both should depend on abstractions. Abstractions should not depend on details.  Details should depend upon abstractions. 

## Tightly Coupled Objects
- Individual objects should be designed so that they can stand alone as much as possible. 
- Coupling between modules occurs when one module directly references another module. In other words, one module “knows” about another module.
- You should aim to [[testing-concepts#Reduce Coupling|reduce coupling]] 