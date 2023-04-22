## Terminology
- Deterministic Algorithm : Identical behavior for different runs for a given input. 
- Randomized Algorithm : Behavior is generally different for different runs for a given input.
	1. World behaves randomly: Consider traditional algorithms that confront randomly generated input. This corresponds to average-case analysis. 
	2. Algorithm behaves randomly: The algorithm makes random decisions that produce varying output for the same input. 

## Deterministic Algorithm
### Quicksort Review
```
- Partition into two lists based on a chosen "pivot" element. list1 = elements smaller than chosen element; list2 elements greater than the chosen element. 
- Call quicksort on each of the lists. 
```

`T(n) <= 2T(n/2) + PartitionTime(n)`
- Best case: `T(n) = O(nlogn)`
- Almost worst case: 9:1 ratio partitions => `O(nlogn)`, as long as the split is of constant proportionality. 
- In the worst case, the list is already sorted an each partition will be `list1=empty` and `list2=everything`. This is the only case where the algorithm runs in O(n^2) time. 

- The behavior of quicksort depends on the relative ordering of the values in the array elements given as the input, and not by the particular values in the array.
- On a random array, the partitioning will be more or less balanced depending on the level. The average running time is therefore `O(nlogn)`

#### Quicksort Randomized
- We want to make the running time independent of input ordering. 
=> Switch the pivot element with a random element. 

*Average-case analysis:*
Random variable X = total # of comparisions for all `Partition` calls. 
`O(nlogn)`

## Random Algorithms
### Gobal Min Cut
- Given a connected undirected graph `G=(V,E)`, find the cut with the minimum cardinality. 
	- A cut parititions the nodes of `G` into two nonempty subsets. 
	- The cardinality (aka size) of the cut is the number of crossig edges which connect one subset to the other. 
		- So we want to find a cut that minimizes the number of crossing edges (there could be several valid min cuts). 
- Ford-fulkerson can give you a min cut, but you don't know if it's the *gobal* min cut. The fastests deterministic algorithm runs in `O(n^3)`. However, it can be solved with a random algorithm!

#### Contraction algorithm
- Works with a connected multigraph (ie an undirected garph that can have multiple "parallel" edges between the same pair of nodes).
- Contraction algorithm: collapse pairs of connect nodes into "supernodes". 

![[Pasted image 20230421112412.png]]

Example:
![[Pasted image 20230421112644.png]]
- The number of edges connecting the final supernode is your global min-cut. However, we got lucky to never contract the middle three edges. 
	- Sometimes the algorithm works, sometimes not. 
	- The contraction algorithm contracts an edge from the min cut with `probability=2/n`
	- So the probability of failure is at most `1/n^2`
=> If you run the contraction algorithm `n^2` times, the probability of finding the min cut is really close to 1. 
Hence we can say the contraction algorithm is `n^2 * O(n) = O(n^3)`
But you can make some more optimizations to make it faster. And the implementation is a lot less complex than Ford-Fulkerson. 
