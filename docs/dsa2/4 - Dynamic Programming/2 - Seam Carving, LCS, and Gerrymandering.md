# 2 - Seam Carving, LCS, and Gerrymandering

## Seam Carving
- Cropping: removes a “block” of pixels
- Scaling: removes “stripes” of pixels
- Seam Carving: removes “least energy seam” of pixels, resizing that doesn’t scale/crop the image
![[Screenshot 2025-03-27 at 12.12.01 AM.png | center | 400]]

### Energy of a Seam
- Sum of the energies of each pixel
	- $e(p)$ = energy of pixel $p$
- Many choices for pixel energy
	- Change of gradient (how much the color of this pixel differs from its neighbors)
	- Shortest path (more color change means more distance)
	- Particular choice doesn’t matter, we use it as a “black box”
- Restriction: seam pixels go up or diagonally up, not to the side
- Goal: find least-energy seam to remove

### 1. Identify the Recursive Structure of the Problem
- Let $S(i,j)$ = least energy seam from the bottom of the image up to pixel $p_{i,j}$
- Assume we know the least energy seams for all of row $n-1$ (i.e. we know $S(n-1,l)$ for all $l$)
- Want to delete the least energy seam going tom bottom to top, so delete $min^m_{k=1}(S(n,k))$
![[Screenshot 2025-03-27 at 12.23.42 AM.png | center | 400]]

![[Screenshot 2025-03-27 at 12.24.01 AM.png | center | 350]]

### Bring It All Together
![[Screenshot 2025-03-27 at 12.37.51 AM.png | center | 500]]

### Repeated Seam Removal
- only need to update pixels dependant on the removed seam
- $2n$ pixels change → $O(2n)$ to update pixels, $O(n+m)$ to find min+backtrack

## Longest Common Subsequence
- Given 2 sequences $X$ and $Y$, find the length of their longest common subsequence
- Brute force: compare every subsequence of $X$ with $Y$, $\Omega(2^n)$

### 1. Identify the Recursive Structure of the Problem
![[Screenshot 2025-03-27 at 1.02.13 AM.png | center | 500]]

### 2. Top-Down Solution with Memoization
- `LCS-Length(X,Y)` sets up memory
- `LCS-recur(X, Y, M, i j)` is the recursive function
![[Screenshot 2025-03-31 at 11.38.53 PM.png | center | 500]]

### 3. Solve in a Good Order
- To fill in cell ($i,j$) we need cells ($i-1,j-1$), ($i-1,j$), and ($i,j-1$)
- Fill from top → bottom, left → right
![[Screenshot 2025-03-31 at 11.54.50 PM.png | center | 400]]

#### LCS Length Algorithm: Bottom-Up
```Python
LCS-Length(X,Y)    # Y for M's rows, X for its columns
	n = length(X)    # get the length of X
	m = legnth(Y)    # get the length of Y
	for i = 1 to n: M[i,0] = 0    # special case: X0
	for j = 1 to m: M[0,j] = 0    # speical case: Y0
	for i = 1 to n:    # for all Xi
		for j = 1 to m:    # for all Yj
			if (X[i] == Y[j])
				M[i,j] = M[i-1, j-1] + 1
			else
				M[i,j] = max( M[i-1, j], M[i, j-1] )
	return M[n,m]    # return LCS length for Y and X
```

- Runtime: $O(n \cdot m)$

### Reconstructing the LCS
1. Start from bottom right
2. If symbols matched, print that symbol then go diagonally
3. Else go to largest adjacent
![[Screenshot 2025-04-01 at 12.02.38 AM.png | center | 400]]

![[Screenshot 2025-04-01 at 12.03.11 AM.png | center | 400]]

## Gerrymandering
- Manipulating electoral district boundaries to favor one political party over others
- Gerrymandering cannot be used to disadvantage racial/ethnic/religious groups, but can be used to disadvantage political parties

### How does it work?
- States are broken into precincts, all of the same size
- Voting preferences of each precinct are known
- Group precincts into districts to maximize the number of districts won by a certain party
![[Screenshot 2025-04-01 at 12.07.06 AM.png | center | 400]]

### Problem Statement
- Given:
	- A list of $n$ precincts: $p_1,p_2,…,p_n$
	- Each containing $m$ voters
	- Total of $m \cdot n$ voters
- Output:
	- Districts $D_1, D_2 \in \{p_1,p_2,…,p_n\}$
	- Where $|D_1| = |D_2| = \frac{m \cdot n}{2}$
	- $S(D_1) > \frac{m \cdot n}{4}$ and $S(D_2) > \frac{m \cdot n}{4}$
		- $S(D_1)$ is the number (size) of voters of the target party in $D_i$
		- $S(D_1) > \frac{m \cdot n}{4}$ means the target party holds the majority in $D_i$
	- “Failure” if no possible solution

### Consider the last precinct
![[Screenshot 2025-04-01 at 12.12.17 AM.png | center | 600]]

### 1. Define Recursive Structure
- $S(j, k, x, y) =$ True if:
	- from the first $j$ precincts
	- $k$ are assigned to $D_1$
	- exactly $x$ vote for R in $D_1$
	- exactly $y$ vote for R in $D_2$

#### Two Ways to Satisfy $S(j,k,x,y)$
![[Screenshot 2025-04-08 at 4.40.47 PM.png | center | 600]]

### Final Algorithm
- True entry: all $n$ precincts have been assigned, $\frac{n}{2}$ precincts are in $D_1$, majority vote in both precincts 
![[Screenshot 2025-04-08 at 4.43.51 PM.png | center | 500]]

### Runtime
- Overall runtime: $O(n^4m^2)$
- Input: list of precincts (size $n$), number of voters (integer $m$)
- Runtime depends on the value of $m$, not the size of $m$
	- Run time is exponential in size of input
		- Input size is $n + |m| = n + log(m)$
- Note: Gerrymandering is NP-Complete
![[Screenshot 2025-04-08 at 4.46.08 PM.png | center | 400]]
