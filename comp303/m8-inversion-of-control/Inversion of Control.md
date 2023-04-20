# Inversion of Control
Inversion of control = reverse the usual control flow from caller code to called code. 
- This helps us achieve better decoupling and separation of concerns

Motivation: you want to keep some stateful objects consistent with one another (eg. for example, keeping the GUI in sync with the class state).

One method for doing this is *pairwise dependencies*, but this has the downsides of high coupling and low extensibility:
![[Pasted image 20230329083924.png]]
If you add more objects, the number of dependencies increases quadratically, which will make for increasingly messy and convoluted code.


## Mode-View-Controller Decomposition (MVC)
This is a high-level design pattern, so it's sometimes referred to as an "architectural pattern".

Goal: We have multiple interdependent objects. We want these objects to be sensitive of each other's state changes.
ie. we want to synchornize the state of the object collection all the objects. 

MVC:
- Make three separate abstractions: 
	- *Model* object for handling data. Receives data from controller, notifies view objects about updates. 
	- *View* object for handling presentation of data
	- *Controller* object acts as a client to the model and view objects, telling them what to do. It handles input from the user. 
[YT explanation](https://youtu.be/DUg2SWWK18I "Share link")

These concept doesn't have one concrete implementation. You can make one object and make 3 interfaces for the abstractions, or make 3 separate objects.
The [[Observer Design Pattern]] is an example of a concrete implementation. 