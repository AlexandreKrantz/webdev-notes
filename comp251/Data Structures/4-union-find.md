### Equivalence Relation 
- An equivalence relation has symmetric and transitive properties. 
- Equivalence relations parition a set into disjoint equivalence classes. 
- For an undirected graph, the connections between vertices are an equivalence relation. 

### Disjoint Set ADT (aka union-find)
Basically like a set of equivalence classes. Each equivalence class is a set, with an unique name (representative member). 

`find(i)` returns the representative of the set that contains i
`sameset(i,j)` returns the boolean value `find(i) == find(j)`
`union(i,j)` merges the sets containing `i` and `j` and returns a new union set

#### Operations & Implementation
- You can implement a disjoint set *find* using something like a table with the elements one column and the representative its set in the other column. 

##### Table Implementation
- Implement union by going through all the elements and updating the representatives of one of the concerned sets with the representative of the other set. 
	- Eg. `union(2,6)` => change all the elements that have the same representative as 6 to have the same representative as 2.  
		- O(n) running time... so it's sorta slow.

##### Forest & Tree Representation
- Each set is represented by a rooted tree. So you have several disjoint rooted trees.
	- The root nodes are the representative. 
	- Each node points to its parent. 
- That way, for the `find` operation you just need to climb up the tree.
- For `union`, you climb up the tree to the root and then fuse the parent to the other tree. 
	- **Size control:** Merge tree with smaller number of nodes into the tree with the largest number of nodes.
![[Pasted image 20230421162632.png]]

*Definitions:*
- The depth of a node is the number of edges from the node to the tree's root node. A root node will have a depth of 0. 
- The height of a node is the number of edges on the longest path from the node to a leaf. A leaf node will have a height of 0

*Runtime:*
- Worst case: only one long tree branch. O(n)
- However, if you do union *by size* or *by height* (ie. fusing shorter tree into taller tree), then you get **O(logn)** for both `find` and `union` operations. 

##### Path Compression
- When we do the `find` operation, it would be better if every node in the tree pointed to the root. This way we don't spend time traversing the "find path". 
![[Pasted image 20230421164309.png]]
[you get near-constant time?]

```
find(i) { 
	if p[i] == i { 
		return i; 
	} else { 
		p[i] = find(p[i]); 
		return p[i]; 
	} 
} 
```