## Why Composition?
Software systems are the result of many interlinked parts. 
These small parts are connected via two mechanisms: 
- Composition: one object holds a reference to another object and gives it certain tasks.  
- [[Inheritance]] (covered in m7)

### Divide and Conquer
- Separation of concerns, and the process of defining larger abstractions in terms of smaller ones (eg. class, object, method), is an example of the *divide and conquer* strategy.
- We solve small problems with the individual pieces and then assemble them together to solve the larger problem. 

### God Class
- A God Class is too big for its own good. Too many methods/fields that should be separated have access to each other, and this violates design principles. 
- To avoid God Classes, we must practice *delegation*. Certain isolated functionalities/services are delegated to a new, separate class. 

### UML Representation
- To represent composition in UML, use a white diamond decoration instead of an arrowhead. The "arrow" points to the class the stores the reference (aka the "parent"). 
- Composition is a stronger form of *aggregation*, which we won't cover hear. We treat them as one and the same. 

We don't want to be arbitrarily storing and passing around references. 
Instead, we use special composition design patterns: 
- [[Composite Design Pattern]]