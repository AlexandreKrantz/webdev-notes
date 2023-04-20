### Conditions for Bipartite Graph
- Vertices are partitioned into 2 sets
- All edges connect vertices from different sets

*Why?*
- Good for representing a mapping a data point with different attributes. For example: `set A = courses; set B = students`.

*Check if graph is bipartite:*
1. Run DFS and use it to build a DFS tree. 
2. Color vertices by layers (e.g. red & black).
3. If all non-tree edges join vertices of different color, then the graph is bipartite.

- A **matching** is a subset of a bipartite graph such that no two edges share a vertex. 
- If a bipartite graph has *n* edges and both A and B have *n* vertices, then we say it is a **perfect matching**.
- **Complete** bipartite graph: any vertex in A is linked to every vertex in B (ie. all possible pairs `(a,b)` are an edge).  

### Stable Matching Problem
1. Each element of A lists elements of B in order of preference. Elements of B do the same for elements of A. 
2. Draw matchings (edges) between elements of A and B. Each element has exactly one edge. Let this matching (set of edges) be `M`. 
	- We say an unmatched pair `(a,b)` is "unstable" if `a` and `b` prefer each other to their current match.
	- If `a` and `b` are self-interested, they could each improve by "escaping" their current match. 
- **Stable matching** = matching with no unstable pairs
	- We say an unmatch  is unstable if an element does not prefer the other. 

**Stable matching problem:** Given the preference lists for all the elements in a bipartite graph, find a stable matching if it exists. 

#### Gale-Shapley Algorithm
- Loop: Each A member offers to most preferred B member, if rejected it moves on to next choice.
	- Each B member accepts the first offer recieved from an A. It also accepts the offer if it is better than the offer it currently has (and rejects current offer). 
*Note:* there is an asymmetry between A and B. B members get to ultimate choose their preferred A. 

- Time complexity: `O(n^2)`
	- Each time through the while loop a candidate applies to a new company. There are only n^2 possible matches.
