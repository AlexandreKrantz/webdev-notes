[Red-Black Trees - Data Structures - YouTube](https://www.youtube.com/watch?v=ZxCvM-9BaXE)

- Balanced bst have guaranteed height of O(log(n)) for n items; so the operations are also O(log(n)).
- Red-black tree is a specific type of balanced bst. 

Conditions:
- Root and leaves are always black (leaves are the nil nodes, so they don't really matter)
- **All red nodes can only have black children**
- All leaves have the same black height

=> all paths from a node to its NIL descendants contain the same number of black nodes (= black height)

Operations: (all have time complexity O(log(n)))
- Search: same as reg bst
- Insert: [Insertion for Red-Black Trees ( incl. Examples ) - Data Structures - YouTube](https://www.youtube.com/watch?v=JwgeECkckRo&t=0s)
- Remove: [Deletion for Red-Black Trees ( incl. Examples ) - Data Structures - YouTube](https://www.youtube.com/watch?v=_c30ot0Kcis)

Note:
- Nodes require one storage bit to keep track of color
- Longest black height in the tree is no more than twice the shortest black height

