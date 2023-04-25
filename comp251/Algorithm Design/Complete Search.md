## Complete Search
- Search up to the entire search space for a solution. 
- Applies only when search space is finite. 

- Programs that generate all possibilities and then choose the ones that are correct are called **filters**.
-  Programs that hone in exactly to the correct answer without any false starts are called **generators**.
- Generally, filters are easier to code but run slower. Do the math to see if a filter is good enough.
- Best practice is to prune the infeasible search space early. 
	- For example, by recognizing symmetric (equivalent) solutions. 

### Recursive Backtracking
- Backtracking can be described as an organized exhaustive search which often avoids searching all possibilities.
- Backtracking can be visualized using a state space tree. 

[6 Introduction to Backtracking - Brute Force Approach - YouTube](https://www.youtube.com/watch?v=DKCbsiDBN6c)

*Backtracking method template:*
```
If at a solution, report success 

for (every possible choice from current state / node) 
• Make that choice and take one step along path 
• Use recursion to try to solve the problem for the new node / state 
• If the recursive call succeeds, report the success to the previous level 
• Back out of the current choice to restore the state at the beginning of the loop. 

Report failure
```