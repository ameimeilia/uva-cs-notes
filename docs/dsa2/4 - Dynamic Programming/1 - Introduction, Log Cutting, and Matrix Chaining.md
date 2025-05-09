---
layout: default
title: Introduction, Log Cutting, and Matrix Chaining
parent: Dynamic Programming Notes
nav_order: 1
---
# Introduction, Log Cutting, and Matrix Chaining
Requires **Optimal Substructure**
	- Solution to larger problem contains the (optimal) solutions to smaller ones

> [!EXAMPLE] Approach
> 1. Identify recursive structure of the problem
> 	- What is the “last thing” done?
> 2. Save the solution to each subproblem in memory
> 3. Select a good order for solving subproblems
> 	- “Top Down”: solve each recursively
> 	- “Bottom Up”: iteratively solve smallest to largest, since recursive calls happen in a predictable order

## Generic Top-Down DP Solution
```Python
mem = {}
def myDPalgo(problem):
	if mem[problem] not blank:
		return mem[problem]
	if baseCase(problem):
		solution = solve(problem)
		mem[problem] = solution
		return solution
	for subproblem of problem
		subsolutions.append(myDPalgo(subproblem))
	solution = OptimalSubstructure(subsolutions)
	mem[problem] = solution
	return solution
```

## Log Cutting
- Find the best way to cut the log, given a log of length \( n \) and a list (of length \( n \)) of prices \( P \) where \( P[i] \) is the price of a cut of size \( i \)
- Select a list of lengths \( l_1,…,l_k \) such that \( \sum l_i = n \) to maximize \( \sum P[l_i] \)
- Brute force runtime: \( O(2^n) \)
- Greedy algorithms build a solution by selecting the most profitable cut first, which may not be the most optimal

### 1. Identify Recursive Substructure
![[Screenshot 2025-03-26 at 9.01.59 PM.png | center | 400]]

### Log Cutting Pseudocode
- Bottom-up approach
- Runtime: \( O(n^2) \)
```Python
initialize memory for profit, choices
cut(n):
	profit[0] = 0
	for i=1 to n:    # log size
		best = 0
		for j=1 to i:    # last cut
			if best < profit[i-j]+P[j]:	
				best = profit[i-j]+P[j]
				choices[i] = j    # gives the size of the last cut
		profit[i] = best
	return profit[n]
```

### Reconstruct the Cuts
- Backtrack through the choices
![[Screenshot 2025-03-26 at 9.21.19 PM.png | center | 400]]

```Python
i = n
while i > 0:
	print choices[i]
	i = i - choices[i]
```

## Matrix Chaining
- How many arithmetic operations are required to multiple a \( n \times m \) matrix by a \( m \times p \) matrix?
	- \( m \) multiplications and \( m - 1 \) additions per element
	- \( n \cdot p \) elements to compute
	- Total cost: \( O(m \cdot n \cdot p) \)
- Given a sequence of matrices (\( M_1, M_2, …, M_n \)) what is the most efficient way to multiply them
![[Screenshot 2025-03-26 at 9.38.45 PM.png | center | 400]]

### 1. Identify the Recursive Structure of the Problem
- \( Best(1,n) \) = cheapest way to multiply \( M_1 \) through \( M_n \)
	- Let \( k \) be the last multiplication that is made
		- \( k \) represents the \( \times \) symbol after matrix \( M_k \)
	- For \( Best(1,j) \):
		- We multiply \( M_i \) through \( M_k \)
		- We multiply \( M_{k+1} \) through \( M_j \)
		- We multiply those two results together

![[Screenshot 2025-03-26 at 9.42.11 PM.png | center | 400]]

### 2. Save Subsolutions in Memory
![[Screenshot 2025-03-26 at 9.43.08 PM.png | center | 400]]

### 3. Select a Good Order for Solving Subproblems
![[Screenshot 2025-03-26 at 9.49.37 PM.png | center | 500]]

### Runtime
![[Screenshot 2025-03-26 at 9.55.48 PM.png | center | 500]]

### Backtrack to Find the Best Order
- “Remember” which choice of \( k \) was the minimum at each cell
- Maintain table `choice[i,j` in addition to `Best` table
	- `choice[i,j] = k` means the best “split” was right after \( M_k \)
	- Work backwards from value for whole problem, `choice[1,n]`
	- Note: `choice[i,i+1] = i` because there are just two matrices

![[Screenshot 2025-03-26 at 10.00.31 PM.png | center | 500]]
- `choice[1,6] = 3`. So  \( [M_1 × M_2 × M_3 ] × [M_4 × M_5 × M_6 ] \)
- We then need `choice[1,3] = 1.` So \( [M_1 × (M_2 × M_3 )] \)
- Also need `choice[4,6] = 5`. So \( [(M_4 × M_5 ) × M_6 ] \)
- Overall: \( [M_1 × (M_2 × M_3 )] × [(M_4 × M_5 ) × M_6 ] \)

## Making Change

### 1. Identify the Recursive Structure of the Problem
- \( Change(n) \): minimum number of coins needed to give change for \( n \) cents
![[Screenshot 2025-03-26 at 10.28.50 PM.png | center | 500]]
- Base case: \( Change(0) = 0 \)
- Running time: \( O(k \cdot n) \) where \( k \) is the number of coins (not efficient)