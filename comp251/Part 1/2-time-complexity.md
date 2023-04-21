Estimate the runtime of a recursive algorithm: 

if you decrease the size of the problem linearly with each recursive call, then the time complexity is `T(n) = T(n/2) + C`, where `C` is the constant time taken for the base case. 

Solve the recurrence:
- we deal with 3 different methods
	1. Guess the solution using skills and talent 
	2. Use induction to show that the solution works
		1. Get in the building
		2. Go up the stairs
		3. Don't lose the context
For practice, check out Langer's exercises: [Michael Langer (mcgill.ca)](https://www.cim.mcgill.ca/~langer/250.html)
[E13-recurrences.pdf (mcgill.ca)](https://www.cim.mcgill.ca/~langer/250/E13-recurrences.pdf)

```
T(n) = c + T(n/2)
	 = c + c + T(n/4)
	 = c + c + c + T(n/8)
	 ...
	 = c*k + T(n/2^k)
	 = c*log2(n) + T(1) 
	 = c*log2(n) + c 

// logarithmic time because we divide by two each time
```

Running time - binary search
- best case (big omega) = 1
- worst case (big O) = log2(n)

the constant factors will depend on the actual implementation. 
make simplifying assumptions about the runtime of the primitive operations. Primitive operations always run in constant time: 
- Assigning a value to a variable
- Calling a method
- Returning from a method
- Arithmetic operations on primitive types
- Comparisons on primitive types
- Conditionals
- Indexing into an array
- Following an object reference
We assume that the running time of all the primitive operations is roughly the same.
Goal: express how t(n) *grows* as we make n very large (asymptotic behavior)
[formal def of big O]

A *tight* upper bound is big O. A *not necessarily tight* upper limit is **small o**. 

