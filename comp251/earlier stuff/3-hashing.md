Beautiful afternoon

### Introduction
- Map is a set of `(key, value)` pairs.
- Each key maps to at most one value. 
- Operations: insert, delete, search.
- Eg. Password authentication: keys are usernames, values are passwords. 
- Efficiency: under reasonable assumptions, the average time to search for an element is O(1). 
	- Insert and delete is also O(1) on average, but this is more common for other data structures. 
- Accessing an array element is also O(1), but this is "direct address". You know the address and store element in that index. Hash tables are different because you don't have a sequence or index. 

### Hash functions
We want to retain the average case O(1) while not using too much space. 

- Each key is equally likely to hash to any of the m slots.
- Similar keys in U should map to different values (for security purposes).
	- U = universe (all possible keys) 
- hash coding = convert keys (from U) into integers
- hash values = convert integers into array indices
keys -> hash code -> hash value

- division method: `h(k) = k * mod(d)`
	- choose d carefully! If d is a power of two and the keys are even, then the odd slots are never used.
	- Rule of thumb: choose d prime and not too close to a power of 2. 
	- Simple method to implement, but you must be careful about choice of d. 
- ??? multiplication method: `h(k) = [m(ks * mod(1)]`
	- More complex method, but the value of m is not as important. 

- Universal hashing: hackers can figure out your hash function and then create keys that always map to the same slot, giving worst-case behavior. 
	- To protect against this, use a different hash function every time, selected randomly. 


### Collision resolution
#### Chaining method: (doubly linked list)
- place all the elements that hash to the same slot into the same linked list. 
- insert at the head of the list, store the key so we can compare when searching. 
- search and delete functions have runtime proportional to the the length of the list. 
- **load factor**: a=n/m <-- assumes elements are distributed uniformly among m slots. 
- average chase analysis: expected time of a search is O(a + 1)
	- a is the traversal time of the average length of the lists, which turns out to be equal to the load factor. 
- successful search: the probability that a list is searched is proportional to the number of element it contians. 

#### Open addressing 
- Maximum 1 key/value stored per slot (no chaining)
- More slots needed than keys 
- Initially, you try the hash function with the probe=0. If there is a collision, you try the hash function again with probe=1, etc, until you find an empty slot. Insert the key/value at this empty slot. 
	- Searching will also use the same prob sequence, so you will have to run the hashing function multiple times. 
- Linear probing: `h(k,i) = (h'(k) + i) mod m`
	- `i` is the **probing number**
	- In fact, the probing number `i` is just incrementing by 1, so you are looking at adjacent slots. BUT, this will create "**primary clusters**", which are long runs of occupied slots.
- To avoid primary clusters, we can do ***quadratic probing***. However, the issue is still there to a lesser degree. `h(k,i) = (h'(k) + i*c1 + i^2*c2) mod m`
- Even better is ***double hashing***  `h(k,i) = (h1(k) + i*h2(k)) mod m`
	- The first hash function is used to compute the initial hash value, and the second hash function is used to compute the step size for the probing sequence.
- Thm1. Expected num of probes in an unseccesssful search is at most `1/1-a`
- Thm2. Expected num of probes in successful search is at most `1/a * log(1/(1-a))*`
- **Unsuccessful search** = searching for an element that is ***not*** in the hash table
- **Successful search** = searching for an element that is in the hash table
- We always assume *uniform hashing* 
- 
