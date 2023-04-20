If S is a subtype of T, then objects of type T may be replaced with objects of type S without altering any of the desirable properties of the program.
*"S is a T"*

The LSP basically states that subclasses should not restrict what clients of the superclass can do with an instance. Specifically, this means that methods of the subclass: 
• Cannot have stricter preconditions; 
• Cannot have less strict postconditions;
• Cannot take more specific types as parameters; 
• Cannot make the method less accessible (e.g., from public to protected); 
• Cannot throw more checked exceptions; 
• Cannot have a less specific return type.