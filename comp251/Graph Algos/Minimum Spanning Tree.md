## Minimum Spanning Tree (MST)
*Context:*
- Say you have an undirected graph `G = (V, E)`
- And each edge `(u,v)` has a weight `w(u,v)`

*Goal:*
- You want to find T ⊆ E such that 
	- T connects all vertices in V (ie. T is a **spanning tree**)
	- The sum of the weights of T is minimized

*Generic Algorithm*
```
MST(V,E)
let T be an empty set

while T does not span V
do:
	find an edge (u,v) safe for T
	add edge (u,v) to T

return T
```
- Do a cut such that the graph is partitioned into elements in T and elements not in T. Choose the edge with the smallest weight that crosses the cut. This edge is **safe for** T.
- Invariant condition: "T is some subset of the MST".
	- Proof:
		- ![[Pasted image 20230419184900.png]]

### Kruskal's Algorithm
1. Start by assigning each vertex its own "component".
2. Finds the smallest weighing edge between two separate components, and fuse those components together.
	- That edge gets added to the MST, which we call `A`

Implementation uses [[8-union-find]] ADT.
![[Pasted image 20230419191041.png]]

Time complexity: `O(E * log(V))`

### Prim's Algorithm 
- Initalize MST as the empty set `A`

1. Start from an arbitrary root vertex `r`
	- Add it to the MST
2. Cut the graph into `V_a` and `V - V_a`, 
	- where `V_a = vertices that A is incident on`
3. Add the smallest weighing edge that crosses the cut to `A`

- This constructs a single tree, which eventually becomes the MST

Example:
![[Pasted image 20230419193440.png]]

- Time complexity (for an implementation using Fibonacci heaps): `O(ElogV)`