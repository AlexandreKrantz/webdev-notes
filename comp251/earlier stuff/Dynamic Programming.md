- In a nutshell, dynamic programming is recursion without repetition. 
- Dynamic programming algorithms store the solutions of intermediate subproblems, often but not always in some kind of array or table.

How to develop your solution:
1. Formulate the problem recursively
	- First, describe exact what the problem is that you want to solve.
	- Give a clear recursive formula or algorithm for the whole problem in terms of the answers to smaller instances of exactly the same problem.
2. Build solutions to your recurrence from the bottom up. Write an algorithm that starts with the base cases of your recurrence and works its way up to the final solution, by considering intermediate subproblems in the correct order.
	- Find a data structure that can store the solution to every subproblem you identified in step (a). This is usually but not always a multidimensional array.