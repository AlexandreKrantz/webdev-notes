### Heaps
• Tree-based data structure (here, binary tree, but we can also use k-ary trees) 
• Max-Heap 
	• Largest element is stored at the root. 
	• for all nodes i, excluding the root, `A[PARENT(i)] ≥ A[i]`. 
• Min-Heap 
	• Smallest element is stored at the root. 
	• for all nodes i, excluding the root, excluding the root, `A[PARENT(i)] ≤ A[i]`. 
• Tree is filled top-down from left to right.

[Heap - Max Heapify - YouTube](https://www.youtube.com/watch?v=5iBUTMWGtIQ)
- Insert the new node as you would for a complete binary tree. 
- When you insert a new node, it may violate the conditions of a heap. 
	- **For each mini subtree, swap the larger of the two child nodes with its parent.**
	- This method is called `MaxHeapify`
	- Worst case runtime: `O(log(n))` , where `n` is the size of the largest subtree. 

*Pseudocode of Max-Heapify:*
1.  First, we assume that the left and right subtrees of the current node are already max heaps.
2.  Then, we compare the value of the current node with the values of its left and right children.
3.  If the value of the current node is greater than or equal to the values of its left and right children, then we don't need to do anything. The current node is already a max heap.
4.  Otherwise, we find the maximum of the current node's left and right children, and swap the current node with that child.
5.  After the swap, we recursively call Max-Heapify on the child node that we just swapped the current node with.

*Building a heap:*
- Use MaxHeapify to convert an array A into a max-heap.
- Call MaxHeapify on each **non-leaf** element in a bottom-up manner.
![[Pasted image 20230421171542.png]]

*Heapsort:*
- Heapsort is a sorting algorithm that uses the heap data structure to sort elements in an array. It is a comparison-based sorting algorithm and has a time complexity of O(nlogn) in the worst case.
Here's how Heapsort works:
1.  Build a max heap from the given array.
2.  Swap the root node (which has the maximum value) with the last element of the array.
3.  Reduce the size of the heap by one and repeat step 2 until the entire array is sorted.
4.  After each swap, run Max-Heapify on the new root to ensure that the heap property is maintained.
**So you basically just poach the root each time, which is the max value of the remaining elements.**

*Runtimes:*
- BuildMaxHeap = O(n) 
	- for loop n-1 times (i.e. O(n)) 
		- exchange elements O(1) 
		- MaxHeapify O(lg n)
- HeapSort O(n lg n)