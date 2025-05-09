---
layout: default
title: MSCS and Chip and Conquer
parent: Divide and Conquer Notes
nav_order: 5
---
# MSCS and Chip and Conquer

## Maximum Sum Continuous Subarray
- the maximum-sum subarray of a given array of integers \( A \) is the interval \( [a,b] \) such that the sum of all values in the array between \( a \) and \( b \) inclusive is maximal
- given an array of \( n \) integers (may include both positive and negative values), give a \( O(nlogn) \) algorithm for finding the maximum-sum subarray

### Divide and Conquer \( \Theta(nlogn) \)
- **Divide**: break the list in half
- **Conquer**: find the best subarrays on the left and right
- **Combine**: find the best subarray that “spans the divide”
![[Screenshot 2025-02-25 at 8.14.31 PM.png | center | 450]]

> [!EXAMPLE] MSCS Divide and Conquer \( \Theta(nlogn) \)
> ```Python
> def MSCS(list):
> 	if list.length < 2:
> 		return list[0]
> 	{listL, listR} = Divide(list)
> 	for list in {listL, listR}:
> 		subSolutions.append(MSCS(list))
> 	solution = max(solnL, solnR, span(listL, listR))
> 	return solution
> ```

## Chip/Decrease and Conquer
- **Divide**: make a subproblem of all but the last element
- **Conquer**: 
	- find the best subarray on the left side \( (BSL(n-1)) \)
	- find the best subarray ending at the divide \( BED(n-1)) \)
- **Combine**
	- new best ending at the divide:
		- \( BED(n) = max(BED(n-1) + arr[n], 0) \)
	- new best on the left:
		- \( BSL(n) = max(BSL(n-1), BED(n)) \)