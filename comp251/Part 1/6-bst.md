# Binary Search Trees
- Keys in left subtree > keys in right subtree
- Operations: search, insert, delete
	- All O(h), where h is the height (= longest path to a leaf) of the bst
- Balanced binary search trees are O(logn) or O(h), but if the bst is unbalanced (one long branch) it can be O(n). 
- In-order traversal of a BST (left child - root - right child), results in visiting the keys in ascending order. 
- Sort an array: build BST tree, then do in-order traversal. 
	- Time complexity = building the tree (inserting n nodes) + traversing the tree = `Ω(nlogn)` in the best case, and `O(n^2)` in the worse case. 
		- If the tree is balanced, building the tree is `n*O(logn)`. Traversing the tree is always `O(n)`.  