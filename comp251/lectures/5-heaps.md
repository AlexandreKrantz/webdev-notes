• Tree-based data structure (here, binary tree, but we can also use k-ary trees) 
• Max-Heap 
	• Largest element is stored at the root. 
	• for all nodes i, excluding the root, A[PARENT(i)] ≥ A[i]. 
• Min-Heap 
	• Smallest element is stored at the root. 
	• for all nodes i, excluding the root, excluding the root, A[PARENT(i)] ≤ A[i]. 
• Tree is filled top-down from left to right.

[Heap - Max Heapify - YouTube](https://www.youtube.com/watch?v=5iBUTMWGtIQ)
- When you insert a new node, it may violate the conditions of a heap. Fix the offending node by exchanging the value at the node with the larger of the values at its children.
	- This method is called `MaxHeapify`
	- Worst case runtime: `O(log(n))` , where `n` is the size of the largest subtree. 


**Building a heap:**
- Use MaxHeapify to convert an array A into a max-heap.
- Call MaxHeapify on each element in a bottom-up manner.