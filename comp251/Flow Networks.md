## Intro to Flow Graphs
- A directed graph can be used to model a flow of material. 
	- The flow is the rate at which material moves. 
	- The edges are like pipes, and the vertices are junctions/intersections of pipes. For any vertex, total flow in must equal total flow out. 
	- The weight of the directed edge is the flow capacity (eg. 200 gallons per hour). 
- We're modelling the movement of material from a *source* vertex $s$ to a *sink* vertex $t$. 
	- Assume that each vertex lies on some path from the source to the sink. 
- The capacity function $c(u,v)$ calculates a positive integer capacity for a given vertex. 
	- The total flow for a vertex must be less than the capacity. 
	- You define the flow out of the source $|f|=X_0$ and then you calculate the flow for each edge $c(u,v)=X_i$ , where $X_0 { \ge } X_i$.

- It's easier to work with flow graphs that have no antiparallel edges, so we create artificial vertices to remove them: 
![[Pasted image 20230402091310.png]]

- Equally, you can convert problems with multiple sinks or multiple sources by adding a *supersink* or *supersource*: 
![[Pasted image 20230402092102.png]]

## Finding Max Flow
### Naive Algorithm
```
Initialize f = 0 
While true { 
	if (∃ path P from s to t such that all edges have a flow less than capacity) 
	then 
		// increase flow along path P up to max capacity 
		β = min{ c(e)-f(e) | e ∈ P} 
		for all e ∈ P { f(e) += β }
	else 
		break 
}
```
- **If we choose the wrong paths to examine, the algorithm will end prematurely and fail at calculating maxium flow**
	- We need to find a way to choose the right paths and not "get stuck". 
![[Pasted image 20230402092836.png]]

### Better Algorithm- The Ford-Fulkerson Method
- Start with initial flow value of 0 for all edges. 
- Residual capacity = remaining capacity. Each edge has a residual capacity `c_f(u,v) = c(u,v) - f(u,v)`. 

- Residual graph `G_f` is composed of the edges that have non-zero residual capacity. 
	- The residual graph may also have reverse edges that are not present in `G`, which is equivalent to decreasing the flow on a particular edge. 
	- Also known as a "residual network"?

- We can push forward on edges with leftover capacity, and we can push backward on edges that are already carrying flow, to divert it in a different direction.

- The maximum amount of flow passing from the source to the sink is equal to the total weight of the edges in a **minimum** cut, i.e., the smallest total weight of the edges which if removed would disconnect the source from the sink.

- An augmenting path is an additional path of flow through the graph that doesn't take any edges above capacity. 
	- Every augmenting path will have a *bottleneck* (the smallest capacity edge along the path). 
- Residual edges are used to *undo* "bad choice" augmenting paths. 

**Very good animated explanation:** [Maximum flow problem - Ford Fulkerson algorithm - YouTube](https://www.youtube.com/watch?v=VbeTl1gG4l4)
Pseudocode: 
```
GET-RESIDUAL-GRAPH(G)
for each edge e = (u,v) in E
	if f(e) < c(e)
	then {
		put a forward edge (u,v) in E_f with residual capacity c_f(e) = c(e) - f(e)
	} 
	if f(e) > 0 
	then {
		put a backward edge (v,u) in E_f with residual capacity c_f(e) = f(e)
	}

AUGMENT-PATH(P)
β = min { c_f (e) | e ∈ P } 
for each edge e = (u,v) ∈ P { 
	if e is a forward edge { 
		f(e) += β
	} else { 
		// e is a backward edge 
		f(e) -= β 
	} 
}

CALCULATE-MAX-FLOW(G)
Initially f(e) = 0 for all e in G
While there is an s-t path in the residual graph G_f
	Let P be a simple s-t path in G_f
	f' = augment(f,P)
	Update f to be f'
	Update the residual graph G_f to be G_f'
Return f
// the algorithm terminates when there is no augmenting path in the residual graph G_f
```

- More detailed explanation: [Ford Fulkerson algorithm for Maximum Flow Problem Example - YouTube](https://www.youtube.com/watch?v=3LG-My_MoWc)
- You can chose an augmenting path through *non-full* forward edges or *non-zero* backwards edges.
 
## Cuts
- A graph cut is a partition of the graph vertices into two disjoint sets. Set A includes the source, set B includes the sink. 
- Calculate the value of the cut by adding the weights (capacity) of the edges that traverse from A to B. 
![[Pasted image 20230402112857.png]]
- You can also calculate the **net flow** across a cut by adding up the flows going from A to B and subtracting the flows from B to A
	- Notation: `|f| = f_out(A) – f_in(A)`
	- All possible net flows are less than or equal to the all possible cut capacities. 
		- Eg. if we exhibit any s-t cut in G of some value c∗, we know that there cannot be an s-t flow in G of value greater than c∗.
		- Furthermore, the maximum flow is equal to the minimum cut. 
![[Pasted image 20230402152445.png]]
![[Pasted image 20230402152458.png]]

If `f` is a flow in a flow network `G = (V,E)` with source `s` and sink `t`, then the following conditions are equivalent:
1. `f` is a maximum flow in `G`
2. The residual netowork `G_f` contains no augmenting paths
3. `|f| = c(S,T)` for some cut `c(S,T) of G`

### Minimum Cut
To find the set A for the min cut, run DFS or BFS on the residual graph of the max flow. The set of reachable vertices defines the set A. 
![[Pasted image 20230402163228.png]]


