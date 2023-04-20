## Graph ADT
- A graph G is composed of a set of vertices V and a set of edges E. Each edge is a pair of vertices.
	- `G = (V,E)`
	- `e = (v1,v2)`
- If the order of the edge pair doesn't matter, it's an directed graph. Else, it's a directed graph. 
- A weighted graph also consists of a a function `W : E -> R` which computes the weight scalar for each edge. 

Some vocab:
- We say the graph is **dense** if the number of edges is roughly equal to the number of vertices squared. 
- If number of edges is much less, we say the graph graph is **sparse**. 
- We say the graph is **strongly connected** if...
	- there's a *path* (not an edge) between every pair of vertices 
	- `|E| >= |V|-1`
- If the graph is stronly connected and `|E| = |V| -1`, then G is a tree

- For a directed graph, each edge `e` has a set of ingoing edges `in(e)` and outgoing edges `out(e)`. 

## Code Representation
- Each vertex has an adjacency list of vertices. 
	- If weighted, store weights in adjacency lists
	- Storage requirement: `O(E+V)`
OR
- Adjacency matrix represents all the edges in the graph in one place (directed graph).
	- Storage requirement: `O(V^2)`. Not memory efficient for large sparse graphs. 
	- In the matrix, you can store numbers corresponding to weights instead of just 0/1. 