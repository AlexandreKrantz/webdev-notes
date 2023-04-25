## Divide and Conquer
- Break problem into smaller sub-problems. The sub-problems should be the same as the original, just on a smaller sample. Combine the solutions of the sub-problems to solve the original problem. 
	- Examples: mergesort, etc. 

*General template:*
```
Recursive in structure 

• Divide the problem into sub-problems that are similar to the original but smaller in size 
• Conquer the sub-problems by solving them recursively. If they are small enough, just solve them in a straightforward manner. 
• Combine the solutions to create a solution to the original problem
```

- Solving divide and conquer recurrences
	- ![[Pasted image 20230422155115.png]]
### Recursive Backtracking + Divide and Conquer
*Example: Optimal binary search trees*
- Running time of OPs in a BST is proportional to the depth of the node we are looking for. 
- In most cases, we want a perfectly balanced tree. But sometimes, there are keys being accessed more frequently than others, so we want to alter the tree to decrease the depth of those keys. 
- Given an array of sorted keys `A[1 .. n]` and an array of corresponding access frequencies `f[1 .. n]`. Our task is to ‘build’ the binary search tree that minimizes the total search time, assuming that there will be exactly `f[i]` searches for each key `A[i]`.
	- The cost to find a given key `i` is `f[i] * depth[i]`
		- Total cost for finding all keys  

### Decrease and Conquer
- Sometimes we’re not actually dividing the problem into many subproblems, but only into one smaller subproblem
	- Example: Binary search