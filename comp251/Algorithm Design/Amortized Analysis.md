### Intro
![[Pasted image 20230419215949.png]]

*Amortization is the process of accounting for an amound over a period*

For CS, we're talking about the **average** performance of each operation in an algorithm in the **worst** case. 

Goal: Show that although some individual operations may be expensive, on average the cost per operation is small.

### Aggregate Analysis
- We show that a sequence of n operations takes worst-case time T(n) in total. In the worst case, the average cost, or amortized cost, per operation is therefore T(n)/n = T(1).
	- Every operation is assign the same amortized cost, even if this doesn't reflect its actual cost. 

*Example:*
k-bit binary counter `A[0, ..., k-1]`.
Counts upwards in binary from 0. 
```
Increment(A,k) // function increments by 1
i = 0
while i<k and A[i]=1
do:
	A[i] = 0
	i = i+1

if i < k
then:
	A[i] = 1
```
Analysis: in the worse case, each call to `Increment` could flip k bits, so incrementing *n* times take `O(nk)` time. 
Since `Increment` is `O(n)`, the average cost per operation is `O(n)/n = O(1)`

### Accounting Method
- Assign different charges to different operations (can be more or less than actual cost).
- Amortized cost = amount we charge
	- When amortized cost is higher than the actual cost, store the different on special "credit" objects. 
	- We need to ensure the credit never goes negative. So therefore the amortized cost must be an upper bound on the total actual cost. 
	- We use the credit later to pay for operations whose actual cost is higher than the amortized cost.

*Example:*
- Charge $2 to set a bit to 1. 
- $1 pays for setting a bit to 1. 
- $1 is prepayment for flipping it back to 0. 
- Have $1 of credit for every 1 in the counter. 
- Therefore, credit ≥ 0.

#### Dynamic Tables
- Say we want to have a table with a constant load factor and we don't know how many objects the application will store in the table. 
	- We want a table that can dynamically adjust its size. 
- Using amortized analysis, we shall show that the amortized cost of insertion and deletion is only `O(1)`, even though the actual cost of an operation is large when it triggers an expansion or a contraction.

*Define:*
- `Table-Insert()` inserts an element into the table.
- `Table-Delete` removes an element, freeing up a slot. 
- let `a'` be the load factor. We want `a'` to be an upper bound on the table's actual load factor. 

For insertion:
- When the table becomes full, double its size and reinsert all existing items. 
	- Guarantees that α ≥ ½. 
	- Each time we insert an item into the table, it is an elementary insertion.

???
![[Pasted image 20230419231311.png]]