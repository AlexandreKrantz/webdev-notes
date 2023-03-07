[Union Find in 5 minutes — Data Structures & Algorithms - YouTube](https://www.youtube.com/watch?v=ayW5B2W9hfo)
Group objects together according to some criteria
Operations:
1. Union (x,y) -> join the groups containing x and y
2. Find(x) -> returns the group that x belongs to 

Graph representation: 
- connect all the objects in the same group with an edge.
- designate a representative element for each group (to be returned by the find function) 
	- two elements belong to the same group iff they have the same representative
- represent each group as a tree (with the representative as the root). To find the representative of an element, keep traveling up the tree by inspecting the parent. 
	- note that we know that we've reach the root when the element's parent is itself (by convention) 
	- to union two groups, set the root of one tree to be the parent of the root of the other. 

Time complexity for finding the root = O(log(n)); this is also the time complexity for the find and union operations. 

### Summary from ChatGPT
A disjoint set data structure, also known as a union-find data structure, is a data structure that tracks a partition of a set into a number of disjoint (non-overlapping) subsets. Each element in the set belongs to exactly one subset.
 
The disjoint set data structure provides two main operations:
1.  MakeSet: creates a new set with a single element.
2.  Union: takes two elements and merges their sets into a single set.
3.  Find: returns the representative element of the set that contains a given element.