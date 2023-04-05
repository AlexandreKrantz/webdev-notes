Inversion of control = reverse the usual control flow from caller code to called code. 
- This helps us achieve better decoupling and separation of concerns

Motivation: you want to keep some stateful objects consistent with one another (eg. for example, keeping the GUI in sync with the class state).

One method for doing this is *pairwise dependencies*, but this has the downsides of high coupling and low extensibility:
![[Pasted image 20230329083924.png]]
If you add more objects, the number of dependencies increases quadratically, which will make for increasingly messy and convoluted code.


## Mode-View-Controller Decomposition (MVC)
This is a high-level design pattern, so it's sometimes referred to as "architectural pattern" or just a "general concept". It can also be thought of as a *decomposition* of concerns, because it's main use is for separation of concerns. 

We want to store pairwise dependencies to synchronize multiple representations of the same data (state). 
- Make three separate abstractions: for *storing* data (model), *viewing* data (view) and *changing* data (control).

These concept doesn't have one concrete implementation. You can make one object and make 3 interfaces for the abstractions, or make 3 separate objects.
The [[Observer Design Pattern]] is an example of a concrete implementation. 