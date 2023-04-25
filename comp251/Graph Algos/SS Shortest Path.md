# Single Source Shortest Path
## Problem Setup
*Input:*
- Directed graph G(V, E)
- Weight function W: E --> R

*Definitions:*
- **Weight of a path** is the sum of the the weight of each edge. 
- **Shortest path** between two vertices is path with the smallest weight (if it exists). If no path exists, then it's defined to be ∞. 

*Problems:*
- Single-source (SSSP): Find shortest paths from a given source vertex s ∈ V to every vertex v ∈ V.
- Single-destination: Find shortest paths to a given destination vertex.
	- By reversing the direction of each edge in the graph, you reduce it to SSSP.
- Single-pair: Find shortest path from u to v.
- All-pairs: Find shortest path from u to v for all u, v ∈ V.
	- Can be solved by running SSSP from every vertex. 

*Note:*
- Some algorithms work only if there are no negative-weight edges in the graph. We must specify when they are allowed and not.
	- If we have a negative-weight cycle, we can just keep going around it, and get w(s, v) = −∞ for all v on the cycle.
- Shortest paths should never contain cycles (path without the cycle is shorter). 
	- Since any acyclic path in a graph G = (V,E) contains at most |V| distinct vertices, it also contains at most |V| - 1 edges. Thus, we can restrict our attention to shortest paths of at most |V| - 1 edges.

*Lemma:*
- Any subpath of a shortest path is a shortest path.
- Proof
	- ![[Pasted image 20230419143105.png]]
	- ![[Pasted image 20230419143121.png]]

## BFS Algorithm
- `d[v]` is the shortest path estimate
- `pi[v]` is a predecessor of `v` on a shortest path from `s` 
	- If v has no predecessor, `pi[v]= nil`
```
SETUP(V, s)
for each v in V:
	d[v] = inf
	pi[v] = nil
d[s] = 0
```
- We say an edge `(u,v)` is *tense* if `d[u] + w(u,v) < d[v]`
	- edge `(u,v)` is *tense*, then `d[v]` must not be the shortest path. It can be *relaxed* by including the `(u,v)` edge. 
```
RELAX(u,v,w)
if d[v] > (d[u] + w(u,v))
then:
	d[v] = d[u] + w(u,v)
	pi[v] = u
```
- Path-relaxation property: 
	- ![[Pasted image 20230419145742.png]]

```
// DAG => no negative weight cycles
// let the graph (V,E) be a DAG

INIT-SINGLE-SOURCE(V,s)
perform setup
topologically sort the vertices
for each vertex u in topological order
do:
	for each vertex v in Adj[u] 
	do: 
		RELAX(u,v,w)
```

#### Time Complexity
- Time complexity = `O(V+E)`
	- ![[Pasted image 20230419153302.png]]

## Dijkstra’s algorithm
- This is a greedy algorithm. 
- Weighted version of BFS. Instead of a regular queue, we use a priority queue. 
	- The priority values are the shortest path estimates `d[v]`.   
- Define `S` to be the set of vertices whose shortest path is determined.
- Define `Q` to be the priority queue of vertices. `Q = V - S`

```
DIJKSTRA(V,E,w,s) 
let S be empty
let Q = V

while Q is not empty
do: 
	u = EXTRACT-MIN(Q) // this just pops off vertex with min d[v] value from Q
	add u to S
	for each vertex v in adj[u]
	do:
		RELAX(u,v,w)
```

#### Time Complexity
Dijkstra's shortest path algorithm is `O(E * logV)` where:
-   `V` is the number of vertices
-   `E` is the total number of edges

#### Loop Invariant Proof
![[Pasted image 20230419161133.png]]