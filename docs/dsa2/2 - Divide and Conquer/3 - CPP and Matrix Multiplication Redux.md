# 3 - CPP and Matrix Multiplication Redux

## Closest Pair of Points

### Naive
- **Given**: a list of points
- **Return**: the pair of points with the smallest distance apart
- **Algorithm**: test every pair of points, then return the closest
- **Running Time**: $O(n^2) → goal is $O(nlogn)$

### Divide and Conquer
- **Divide**: at median $x$ coordinate
- **Conquer**: recursively find closest pairs from LeftPoints and RightPoints
- **Combine**: return smaller of left and right pairs
	- Case 1: closest pair is completely in LeftPoints or RightPoints
	- Case 2: Closest pair spans the division
		- compare all pairs of points within $d = min\{d_L, d_R\}$ of the cut
		- worst case: need to compare all points, $T(n) = 2T(\frac{n}{2}) + \Omega(n^2) \in \Omega(n^2)$
		![[Screenshot 2025-02-23 at 4.17.49 PM.png | center | 300]]
		- reducing search space: divide the runway into squares with dimension $d/2$ so that only at most one point is in a square
		- at most 7 squares can contain a point $<d$ away
		![[Screenshot 2025-02-23 at 4.21.46 PM.png | center | 300]]
- **Conquer**:
	- Construct a list of points in the boundary
	- Sort runway point by $y$-coordinate
	- Compare each point in runway to the $7$ points above is and save the closest point
	- Output the closest pair among left, right, and runway points

- Sorting runway points by $y$ coordinate is an expensive step
- Solution: maintain additional information in the recursion
	- Minimum distance among pairs of points in the list
	- List of points sorted according to $y$-coordinate
	- Sorting runway points by $y$-coordinate now becomes a merge
	- Output closest pair among left, right, and runway points, and $y$-sorted list

#### Listing Points in the Boundary
- LeftPoints:
	- Closest Pair: $(1,5), d_{1,5}$
	- Sorted Points: $[3,7,5,1]$
- RightPoints:
	- Closest Pair: $(4,6), d_{4,6}$
	- Sorted Points: $[8,6,4,2]$
- Merged Points: $[8,3,7,6,4,5,1,2]$
- Runway Points: $[8,7,6,5,2]$

### Running Time
- $\Theta(nlogn)$
- Recurrence: $T(n) = 2T(\frac{n}{2}) + \Theta(n)$
- Case 2 of Master’s Theorem: $T(n) = \Theta(nlogn)$
![[Screenshot 2025-02-23 at 4.34.07 PM.png | center | 500]]

## Matrix Multiplication Redux

### Matrix Multiplication
![[Screenshot 2025-02-23 at 4.39.25 PM.png | center | 500]]

### Matrix Multiplication Divide and Conquer
![[Screenshot 2025-02-23 at 4.42.48 PM.png]]

### Strassen’s Algorithm
- use Karatsuba-like technique
![[Screenshot 2025-02-23 at 4.45.13 PM.png]]
![[Screenshot 2025-02-23 at 4.44.19 PM.png | center | 500]]
