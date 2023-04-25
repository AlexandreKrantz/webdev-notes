### Introduction
- Map is a set of `(key, value)` pairs.
- Each key maps to at most one value. 
- Operations: insert, delete, search.
- Eg. Password authentication: keys are usernames, values are passwords. 
- Efficiency: under reasonable assumptions, the average time to search for an element is O(1). 
	- Insert and delete is also O(1) on average, but this is more common for other data structures. 
- Accessing an array element is also O(1), but this is "direct address". You know the address and store element in that index. Hash tables are different because you don't have a sequence or index. 

### Hash Functions
We want to retain the average case O(1) while not using too much space. 

*Properties of a good hash function:* 
- Each key is equally likely to hash to any of the m slots.
- Similar keys should map to different values (for security purposes).
- hash coding = convert keys into integers (sometime the key is already an integer so this step is not needed).
- hash values = convert integers into array indices.

*Process recap:*
`key (abstract object) -> hash code (integer) -> hash value (array index)`

*Hash functions take us from hashcodes to hashvalues.*
- **Division method**: `h(k) = k * mod(d)`
	- `k = hashcode of key; d <= size of array;`
	- choose d carefully! If d is a power of two and the keys are even, then the odd slots are never used.
	- Rule of thumb: choose d prime and not too close to a power of 2. 
	- Simple method to implement, but you must be careful about choice of d. 
- **Multiplication method**: `h(k) = [m((k*A) % 1]`
	- `A` is a constant between 0 and 1. 
	- `[]` is the floor function, rounding down to the nearest integer. 
	- `m` is the size of the hash table. 
	- More complex method, but the value of m is not as important. 
	- Step-by-step:
		1. Multiply the key k by the constant A.
		2.  Take the fractional part of the product (k * A mod 1). This means taking only the decimal portion of the product and discarding the integer part. For example, if k * A = 3.14159, then the fractional part is 0.14159.
		3.  Multiply the fractional part by the size of the hash table m.
		4.  Take the floor of the result to obtain the hash index. This means rounding down the product to the nearest integer.

- Universal hashing: hackers can figure out your hash function and then create keys that always map to the same slot, giving worst-case behavior. 
	- To protect against this, **use a different hash function every time**, selected randomly. 


### Collision Resolution
#### Chaining method: (doubly linked list)
- place all the elements that hash to the same slot into the same linked list. 
- insert at the head of the list, store the key so we can compare when searching. 
- search and delete functions have runtime proportional to the the length of the list. 
- **load factor**: a=n/m <-- assumes elements are distributed uniformly among m slots. 
- average case analysis: expected time of a search is O(a + 1)
	- a is the traversal time of the average length of the lists, which turns out to be equal to the load factor. 
- successful search: the probability that a list is searched is proportional to the number of element it contains. 

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

*Average case analysis:* (remember, time complexity is always O(1))
- Thm1. Expected num of probes in an unseccesssful search is at most `1/1-a` (decreasing => O(1))
- Thm2. Expected num of probes in successful search is at most `1/a * log(1/(1-a))*`
	- **Unsuccessful search** = searching for an element that is ***not*** in the hash table
	- **Successful search** = searching for an element that is in the hash table
	- We always assume *uniform hashing*. Universal hashing can be approximated by choosing a different hash function from an array of different good hash function each time. 
	- [explanation of expected probes](http://www2.hawaii.edu/~suthers/courses/ics311f20/Notes/Topic-06.html#:~:text=Analysis%20of%20Open%20Addressing)