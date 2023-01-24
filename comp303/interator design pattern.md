Iterating (or enumerating) over the items in a collection. 
SOLID Principles say that a unit should only have a single responsibility (single reason to change). => Iterator should be separate from the objects in the collection. 

**Benefits:** 
- Traverse various ***different types of data structures*** in a consistent manner. "A uniform interface". 
	- Collections in C# and Java (like ArrayLists) implement the iterator pattern for Lists, but you can use it for any structure that you create. 
- ***You don't need to expose the structure***. More security; only one item exposed at a time. Encapsulation and information hiding benefit. 
- Lazy evaluation. 
	- You can stop the computation at any time. 
	- Collection could be infinite. 

Use:
- Extract the iterator: `obj.getIterator`
	- Note: The class of `obj` implements the `Iterable` interface. 

