---
layout: default
title: Review
parent: Graphs Notes
nav_order: 1
---
# Review

> [!INFO] Readings in CLRS 4th edition:
> • CLRS Chapter 2: insertion sort (if needed), book’s pseudocode conventions  
> • CLRS Chapter 3: info on order-class (more than we cover in lecture), math review  
> • Note: book goes into more depth than we do, and topics we don’t need. Use it to  
reinforce what’s taught in lectures.

## Measuring Work
- measure an algorithm’s work in terms of input size by counting some “basic operation”
- **Worst case \( W(n) \)**
	- maximum number of basic operations for any input of size \( n \)
	- upper-bound
- **Average case \( A(n) \)**
	- harder to calculate, needs:
		- amount for work \( T_I \) for every input \( I \)
		- the probability it occurs, \( P_I \)
		- \( A(n) = \sum T_I P_I \)

> [!EXAMPLE] Example. Hash table search, insert, delete
> \( W(n) \): search = \( n \)
> \( A(n) \): search = \( 1 \)

- **Problems** and their properties:
	1. feasible or tractable problems
	2. intractable problems
		- problems that seem to need exponential time complexity, \( \Theta (k^n) \) where \( k \) is a constant \( > 1 \)
		- ex. NP-hard problems, NP-complete problems
	3. unsolvable problems (e.g. the Halting Problem)

	- **lower bound** for the number of operations needed to solve a problem
		- can we prove that it’s impossible for any algorithm to solve this problem in fewer than some number of operations?

## Asymptotic Analysis and Order Classes
![[Screenshot 2025-01-16 at 2.29.18 PM.png | center | 300]]

- **Order Classes**: categorize an algorithm’s complexity
	- find equivalency by looking at **asymptotic growth**
- **Asymptotic Bounds**
	- \( O(g) \): set of all functions that grow **no faster than** \( g \), \( g \) is an **asymptotic upper bound**
		- \( \{ \ functions \ f \ | \ \exists \ constants \ c, \ n_0 > 0 \ s.t. \forall n > n_0, f(n) ≤ c \cdot g(n)\} \)
	- \( \Omega (g) \): set of all functions that grow **at least as fast** as \( g \), \( g \) is an **asymptotic lower bound**
		- \( \{ \ functions \ f \ | \ \exists \ constants \ c, \ n_0 > 0 \ s.t. \forall n > n_0, f(n) ≥ c \cdot g(n)\} \)
	- \( \Theta (g) \): set of all functions that grow at the **same rate** as \( g \), \( g \) is an **asymptotic tight bound**
		- \( \Omega (g(n)) \cap O(g(n)) \)
	- \( o(g) \): set of all functions that **always grow slower** than \( g \)
		- \( \{ \ functions \ f \ | \ \forall \ constants \ c > 0, \ \exists n_0 \ s.t. \forall n > n_0, f(n) < c \cdot g(n)\} \)
	- \( \omega(g) \): set of all functions that **always grow faster** than \( g \)
		- \( f(n) \in \omega(g(n)) \iff \lim_{n\to\infty} \frac{f(n)}{g(n)}= \infty \)

> [!EXAMPLE] Asymptotic Notation Direct Proof Example
> **Show**: \( nlogn \ \in \ O(n^2) \)
> **Technique**: find \( c, \ n_0 > 0 \ s.t. \forall n > n_0, nlogn ≤ c \cdot n^2 \)
> 
> **Proof**: 
> Let \( c = 1, \ n_0 = 1 \). Then,
> 		\( n_0 log n_0 = (1) log (1) = 0, \)
> 		\( c(n_0)^2 = 1 \cdot 1^2 = 1, \)
> 		\( 0 ≤ 1 \)
> 		\( \forall n ≥ 1, \ log(n) < n \implies nlogn ≤ n^2 \)

> [!EXAMPLE] Asymptotic Notation Proof By Contradiction Example
> **Show**: \( n^2 \notin O(n) \)
> **Technique**: Contradiction
> 
> **Proof**: 
> Assume \( n^2 \in O(n) \). Then \( \exists c, \ n_0 >0 \ s.t. \forall n > n_0, \ n^2 ≤ cn \)
> Let us derive constant \( c \). For all \( n > n_0 > 0 \), we know:
> \( cn ≥ n^2 \)
> \( c ≥ n \)
>
> Since \( c \) is dependent on \( n \), it is not a constant. Contradiction. Therefore \( n^2 \notin O(n) \)

## Math Reminders

### Algorithms
1. **Sequential or Linear Search**
	- no assumptions on the order of values in the list
	- compares the target to keys of the list-items
	- always \( \Theta(n) \)
		- worst case = \( n \), average \( n/2 \)
2. **Binary Search**
	- list must be sorted
	- \( \Theta(logn) \) in the worst case
		- reminder: balanced search trees are also \( \Theta(logn) \) in the worst case
3. **Insertion Sort**
	- \( \Theta(n^2) \) in the worst case, \( \Theta(n) \) in the best case
	- in-place sort (extra storage is constant in size)
	- other \( \Theta(n^2) \) worst-case sorts: selection sort, bubble sort
	- CLRS section 2.1 for more info
4. **Merge Sort**
	- divide and conquer, usually implemented recursively on smaller sublists
	- \( \Theta(nlogn) \) in the worst case
	- not in-place, need \( \Theta(n) \) extra storage for the merge
	- CLRS section 2.3.1 for more info
5. **Quick Sort**
	- also divide and conquer
	- \( \Theta(nlogn) \) in the best and average cases
	- \( \Theta(n^2) \) in the worst case, but this can be avoided
	- in-place
	- CLRS chapter 7 for more info