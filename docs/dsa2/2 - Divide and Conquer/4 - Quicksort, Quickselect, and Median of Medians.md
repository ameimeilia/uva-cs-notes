---
layout: default
title: Quicksort, Quickselect, and Median of Medians
parent: Divide and Conquer Notes
nav_order: 4
---
# Quicksort, Quickselect, and Median of Medians

## Quicksort
- like Mergesort, its expected runtime is \( O(nlogn) \) 
- unlike Mergesort, the **divide** step is the most expensive, and it is typically faster than Mergesort
- idea: choose a pivot element, recursively sort two sublists around that element

- **Divide**: select pivot element \( p \), Partition(\( p \))
- **Conquer**: recursively sort left and right sublists
- **Combine**: nothing!

### Partition Procedure
![[Screenshot 2025-02-23 at 4.56.20 PM.png | center | 500]]

### Conquer
![[Screenshot 2025-02-23 at 4.56.50 PM.png | center | 400]]

### Runtime

#### Optimistic
![[Screenshot 2025-02-23 at 4.57.48 PM.png | center | 400]]

#### Worst Case
![[Screenshot 2025-02-23 at 4.58.09 PM.png | center | 400]]

- if the list is nearly sorted
![[Screenshot 2025-02-23 at 4.58.59 PM.png | center | 400]]

#### Choosing a Good Pivot
- good: \( \Theta(nlogn) \), pivot roughly even split between the left and right
- bad: \( \Theta(n^2) \)
- to find the median in linear time, user the Quickselect algorithm

## Quickselect
- Algorithm to compute the \( i^{th} \) order statistic
	- \( i^{th} \) smallest element in the list
	- \( 1^{st} \) order statistic: minimum
	- \( n^{th} \) order statistic: maximum
	- \( (n/2)^{th} \) order statistic: median
- Idea: choose a pivot element, partition around the pivot, and recurse on sublist containing index \( i \)
- **Divide**: select pivot element \( p \), Partition(\( p \))
- **Conquer**:
	- if \( i = \) index of \( p \), then we are done and return \( p \)
	- if \( i < \) index of \( p \) recurse left, otherwise recurse right
- **Combine**: nothing!

### Partition Procedure
![[Screenshot 2025-02-23 at 5.05.47 PM.png | center | 400]]

### Conquer Step
![[Screenshot 2025-02-23 at 5.06.26 PM.png | center | 400]]

> [!EXAMPLE] CLRS Pseudocode for Quickselect
> ```Python
> randomized-select(A,p,r,i)
> if p == r
> 	return A[p]
> q = randomized-partion(A,p,r)
> k = q - p +1    # number of elements in left sub-list + 1
> if i == l    # the pivot value is the answer
> 	return A[q]
> else if i < k
> 	return randomized-select(A,p,q-1,i)
> # note adjustment to i when recursing on right side
> else return randomized-select(A,q+1,r,i-k)
> ```

### Runtime

#### Optimistic
![[Screenshot 2025-02-23 at 5.14.21 PM.png | center | 400]]

#### Worst Case
![[Screenshot 2025-02-23 at 5.14.40 PM.png | center | 400]]

#### Choosing a Good Pivot
- Good: \( \Theta(n) \)
- Bad: \( \Theta(n^2) \)
- Same issue as Quicksort? Solution:
	- Median of medians algorithm that finds something close to the median in \( \Theta(n) \) time
	- Prove that when its result is used with Quickselect’s partition, then Quickselect is guaranteed \( \Theta(n) \) 
		- which then guarantees Quicksort is \( \Theta(nlogn) \)
	- Notes:
		- have to do this for every call to Partition in Quicksort
		- could just use the value returned by median of medians for Quicksort’s Partition

## Median of Medians
- decent pivot: both sides of pivot > 30%
![[Screenshot 2025-02-23 at 5.18.47 PM.png | center | 400]]
- idea: break list into blocks, find the median of each block, then use the median of those medians