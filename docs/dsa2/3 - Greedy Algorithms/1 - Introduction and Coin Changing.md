---
layout: default
title: Introduction and Coin Changing
parent: Greedy Algorithms Notes
nav_order: 1
---
# Introduction and Coin Changing
- **greedy algorithm**: makes the best decision it can at a given point, no undoing or looking ahead
- **optimization problems**:
	- a solution must meet certain constraints:
		- a solution is **feasible**
	- solutions are judged on some criteria 
		- **objective function**: what to do to determine the best decision
	- one or more feasible solution that scores the best is called the **optimal solution(s)**

## Greedy Strategy: An Overview
- build solution by stages, adding one item to the partial solution from the previous stage
- at each stage, make **locally optimal choice** based on the **greedy choice**
- irrevocable: choice can’t be undone
- sequence of locally optimal choices leads to globally optimal solution
- problem must have **optimal substructure**
	- optimal solution to a problem contains optimal solutions to subproblems
	- if A is an optimal solution to a problem, then the components of A are optimal solutions to subproblems
- idea:
	1. identify a greedy **choice property**
		- a choice guaranteed to be included in some optimal solution
	2. repeatedly apply the choice property until no subproblems remain

## Correctness of Greedy Algorithm
- optimal solution must satisfy the following properties:
	- at most 4 pennies
	- at most 1 nickel
	- at most 2 dimes
	- cannot contain 2 dimes and 1 nickel
- **claim**: argue that at every step, greedy choice is part of some optimal solution
- **case 1**: suppose $x<5$
	- optimal solution must contain a penny
	- **greedy choice**: penny
- **case 2**: suppose $5≤x<10$
	- optimal solution must contain a nickel
		- suppose otherwise, then optimal solution can only contain pennies, so it must contain $x>4$ pennies (contradiction)
	- **greedy choice**: nickel
- **case3**: suppose $10≤x<25$
	- optimal solution must contain a dime
		- suppose otherwise, then optimal solution can contain at most 1 nickel, so there must be at least 5 pennies in the optimal solution (contradiction)
	- **greedy choice**: dime
- **case 4**: suppose $25≤x$
	- optimal solution must contain a quarter
		- suppose otherwise:
			1. if it contains 2 dimes, then it can contain 0 nickels, in which case it contains at least 5 pennies (contradiction)
			2. if it contains fewer than 2 dimes, then it can contain at most 1 nickel, so it must also contain at least 10 pennies (contradiction)
	- **greedy choice**: quarter
- **conclusion**: in every case, the greedy choice is consistent with some optimal solution

## Wrap-up on Greedy Basics
- an approach to solving **optimization problems**
	- finds **optimal solution** among set of **feasible solutions**
- works in stages, apply **greedy choice** at each stage
	- makes locally optimal choice, with goal of reaching overall optimal solution for entire problem
- proof needed to show correctness
- remember: problem must have **optimal substructure property**