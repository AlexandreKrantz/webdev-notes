State diagrams represent a static view of the code (based on inheritance). 
Sequence diagrams model a dynamic perspective (based on a specific execution of the code). 

For example if we want to call the `isEmpty()` method of a `CompositeCardSource`, then it is necessary to descend down all the aggregate objects and call their `isEmpty()` method also. 

Example: 
![[Pasted image 20230303184805.png]]
Solid arrows are method calls, the dashed arrows are responses. 

