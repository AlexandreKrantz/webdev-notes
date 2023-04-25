## Searching Algorithms
### Breadth-First Search
- "Colors" the vertices to keep track of progress.
	- White = Undiscovered.
	- Grey = Discovered but not finished. 
	- Black = Finished.
- Input = source vertex s.
- Ouput = shortest path (smallest number of edges) from s to v, where v is a target vertex.
- Initialization = O(V) for reseting the vertices
- Traversal loop = O(V) enqueuing/dequeuing vertices + O(E) for scanning adjacency lists = O(V+E)
- Unweighted directed graph G=(V,E) 

### Depth-First Search
```
scan adjacency list of vertex u, find child v
set v.pi = u 
predecessor subgraph: G_pi = (V, E_pi), where E_pi = {(v.pi,v), for v in V}
```
- vertices are initially white 
- grayed when *discovered* in the search
- blackened when its adjacency list has been examined completely (ie. *finished*).

- "Timestaps" are integers between `1` and `2|V|`. 
	- Used to track when vertices are discovered and finished.  We use store the information in attributes `u.d` and `u.f`
	- `u.d < u.f`

Algorithm works recursively on sub-trees:
``` 
global variable: time

DFS(G){
for each vertex u in G.V
	u.color = white
	u.pi = nil
time = 0
for each vertex u in G.V // explore disjoint trees
	if u.color == white
	VISIT(G, u)
}

VISIT(G, u) {
// u is discovered
time = time + 1 
u.d = time
u.color = grey

// explore adjacent vertices to u
for each v in G.adj[u] 
	if v.color == white
		v.pi = u // set parent attribute
		VISIT(G, v) // visit child
u.color = black
time = time + 1
u.f = time		
}
```
Runtime: O(V + E)
- V because of the `DFS` part explores all nodes
- E because the `VISIT` explores the adjacency lists

- DFS explores nodes in a parenthesis structure between discovery and finishing. 
![[Pasted image 20230326164333.png]]
- If we represent the discovery of vertex u with a left parenthesis "(u" and represent its finishing by a right parenthesis "u)"
![[Pasted image 20230326164432.png]]

Interatrive version of algorithm (simplified)
```
VISIT(s)
stack.push(s)
while stack is not empty
	v = stack.pop
	if v.color = white
		visit v
		for each child w of v
			push(w)
```

#### Classification of Edges - DFS
- An edge (u,v) is a *tree edge* if it was used to discover v. 
- (u,v) is a *back edge* if u is a descendant of v (includes self loops). 
- (u,v) is a *forward edge* if v is a descendant of u, but not a tree edge (ie. it was already discovered via a different edge). 
- any other edge is called a *cross edge*. These can be connecting together different DFS trees, for example. 
![[Pasted image 20230326170210.png]]
- The edge type can also be identified during DFS, based on the color of v (when explore children of u):
	- white => tree edge
	- gray => back edge
	- black => forward or cross edge

![[Pasted image 20230326170759.png]]

## Directed Acyclic Graph
Graph with directed edges and no cycles. 
- *source* = vertex with no incoming edges
- *sink* = vertex with no outgoing edges
Every DAG has at least one source and one sink

- White path theorem: v is a descendant of u iff there is a path from u to v consisting of only white vertices 

- Lemma 1: A directed graph G is acyclic (DAG) iff a DFS of G yields no back edges. 
	- because back edge => cycle 

### Topological Sort
let `G = (V,E)` be a DAG. 
Order the vertices in a line, so all directed edges go from left to right. 
![[Pasted image 20230326175236.png]]

Algorithm:
```
TOPOLOGICAL-SORT(G)
1. call DFS(G) to compute finishing times v.f for each vertex v
2. in the order of finishing times, insert the vertices onto the front of a link list
3. return the linked list of vertices

```
Topologically sorted vertices appear in reverse order of their DFS finishing times. 
Runtime is O(V+E) because it's the same as DFS. 

## Strongly Connected Components
- G is strongly connected if every pair (u, v) of vertices in G is *reachable* from one another.
	- In directed graphs, edges between nodes are one way by default so there will probably be pairs of nodes that can't reach each other. 

- Depth-first search is used to decompose a directed graph into strongly connected components. 
	- Many algorithms that work with directed graphs do this decomposition and then run on each component separately.

Formal definition: 
- A strongly connected component of G is a maximal subset of vertices C in V such that for all u,v in C, paths u -> v and v -> u both exist.

- $G^T$ is the transpose of $G$, defined as the graph you get when you reverse the directions of all the edges. 
	- Both $G$ and $G^T$ have the same stronly connected components. 
![[Pasted image 20230401160742.png]]
Explanation: [Kosaraju algorithm introduction - YouTube](https://www.youtube.com/watch?v=Jb1XlDsr46o)

- If we take each *component* to be a vertex, we can make a **component graph**. This graph is necessarily a DAG, by the following lemma:
![[Pasted image 20230401161859.png]]

![[Pasted image 20230401163036.png]]

