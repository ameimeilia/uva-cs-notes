---
layout: default
title: Introduction
parent: Divide and Conquer Notes
nav_order: 1
---
# Introduction
- Can you cover an 8x8 grid with 1 square missing using “trominoes”?
	- What about larger boards?
- **Solution**:
	1. divide the board into quadrants, then place a tromino to occupy the three quadrants without the missing piece
	2. each quadrant is now a smaller subproblem that can be solved recursively
![[Screenshot 2025-02-18 at 2.23.06 PM.png]]

## Divide and Conquer
- **Divide**: break the problem into multiple subproblems, each smaller instances of the original
- **Conquer**:
	- if the subproblems are “large,” solve each subproblem recursively
	- if the problems are “small,” solve them directly (base case)
- **Combine**: merge solutions to subproblems to obtain solution for original problem

>[!example] Generic Divide and Conquer Solution
>```Python
> def myDCalgo(problem):  
>	if baseCase(problem):  
>		solution = solve(problem) #brute force if necessary  
>		return solution  
>	subproblems[] = Divide(problem)  
>	for subproblem in subproblems:  
>		subsolutions.append(myDCalgo(subproblem))  
>	solution = Combine(subsolutions)  
>	return solution
>```

![[Screenshot 2025-02-23 at 3.41.19 PM.png]]

### Analyzing Divide and Conquer
- **Divide**: \( D(n) \) time
- **Conquer**: Recurse on smaller problems of size \( s_1, …, s_k \)
- **Combine**: \( C(n) \) time
- **Recurrence**: \( T(n) = D(n) + \Sigma_{i\in[k]}T(s_i) + C(n) \)

## MergeSort
- **Divide**: break \( n \)-element list into two lists of \( \frac{n}{2} \) elements
- **Conquer**:
	- if \( n>1 \): sort each sublist recursively
	- if \( n=1 \): list is already sorted (base case)
- **Combine**: merge together sorted sublists into one sorted list

>[!example] MergeSort Divide and Conquer Solution
>```Python
> def mergesort(list):
>	if list.length < 2:
>		return list # list of size 1 is sorted!
>	{listL, listR} = Divide_by_median(list)
>	for list in {listL, listR}:
>		sortedSubSlists.append(mergesort(list))
>	solution = merge(sortedL, sortedR)
>	return solution
>```

>[!example] MergeSort Merge Solution
>```Python
>def merge(L1, L2, Lout):
>	while (L1 and L2 not empty):
>	if L1[0] ≤ L2[0]:
> 		Lout.append(L1.pop())
>	else:
> 		Lout.append(L2.pop())
> 	Lout.append(L1)
>	Lout.append(L2)

![[Screenshot 2025-02-23 at 2.31.23 PM.png | center | 400]]

### Analyzing Merge Sort
- **Divide**: 0 comparisons
- **Conquer**: recurse on 2 small problems, size \( \frac{n}{2} \)
- **Combine**: \( n \) comparisons
- **Recurrence**: \( T(n) = 2T(\frac{n}{2})+n \)