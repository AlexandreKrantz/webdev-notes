### AVL Trees
Super Good: [Data Structure and Algorithms - AVL Trees (tutorialspoint.com)](https://www.tutorialspoint.com/data_structures_algorithms/avl_tree_algorithm.htm)
**Def: BST such that the heights of the two child subtrees of any node differ by at most one (aka a self-balancing binary tree).** 
- Insert, Delete & Search take **O(log n)** in average and worst cases; n = number of nodes in tree
- The **height** of a null node in a bst is 0, and we count the *number of edges between a node and a null node to get the height of that node*. 
- The **balance factor** of a binary tree is the difference in heights of its two subtrees **(hL - hR)**. It may take on one of the values -1, 0, +1.
	- Left-heavy => balance > 1 => right rotation 
	- Right-heavy => balance < -1 => left rotation
	- All BST properties are maintained when rotating
```
INSERT()
perform insertion as you would for BST
check that the magitude of balance factor never exceeds 1
	rebalance the tree as necessary if it does

DELETE()
- perform deletion as you would for BST
	- If the node to be deleted has two child nodes, "promote" the inorder successor. 
- check that the magitude of balance factor never exceeds 1
	- rebalance the tree as necessary if it does
```



#### Outside and Inside Insertions
- Outside insertion on the right => left rotation of subtree
- Inside-right insertion => left-right rotation
	- the first left rotation is on the smallest subtree containing the leaf, the second rotation is on the whole subtree. 
- **All the rotation operations are O(1)**, so the running time of insert in AVL trees is O(h) = O(log(n))
- h = O(log(n)), where n is the number of  nodes.

Apply the rotations when there is a violation of AVL properities (diff in heights of children > 1)
Violations occure when we are inserting a node (according to BST insertion algorithm) 

Inside:
1. rotation of immediate subtree (break the zigzag pattern, but it's still going inside)
2. rotation of parent subtree in other direction (balance the tree) 