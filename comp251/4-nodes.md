### Equivalence Relation 
- An equivalence relation has symmetric and transitive properties. 
- Equivalence relations parition a set into disjoin equivalence classes. 
- For an undirected graph, the connections between vertices are an equivalence relation. 

### Disjoint Set ADT
Basically like a set of equivalence classes. Each equivalence class is a set, with an unique name (representative member). 

`find(i)` returns the representative of the set that contains i
`sameset(i,j)` returns the boolean value `find(i) == find(j)`
`union(i,j)` merges the sets containing `i` and `j` and returns a new union set

#### Operations
- You can implement a disjoint set *find* using something like a table with the elements one column and the representative its set in the other column. 

- Implement union by going through all the elements and updating the representatives of one of the concerned sets with the representative of the other set. 
	- Eg. `union(2,6)` => change all the elements that have the same representative as 6 to have the same representative as 2.  
		- But this algorithm runs in linear time and sucks :(

