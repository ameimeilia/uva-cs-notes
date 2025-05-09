---
layout: default
title: Reductions
parent: Reductions Notes
nav_order: 3
---
# Reductions
- Algorithm technique of supreme ultimate power
- Convert instance of Problem A to an instance of Problem B
- Convert solution of Problem B back to a solution of Problem A

## In General: Reduction / Worst Case Lower Bound
- Definition:
	- A **worst case lower bound** on a problem is an asymptotic lower bound on the worst case running time of any algorithm which solves it
	- If \( f(n) \) is a worst case lower bound for problem \( A \), then the worst case running time of any algorithms which solves \( A \) must be \( \Omega(f(n)) \)
		- i.e. for sufficiently large values of \( n \), for every algorithm which solves \( A \), there is at least one input of size \( n \) which causes the algorithm to do \( \Omega(f(n)) \) steps
- Examples:
	- \( n \) is a worst case lower bound on finding the minimum in a list
	- \( n^2 \) is a worst case lower bound on matrix multiplication
![[Screenshot 2025-04-19 at 9.59.13 PM.png | center | 500]]

## Worst Case Lower Bound Proofs
- Problem \( A \) is easier than Problem \( B \): \( A ≤ B \)
	- The name “reduces” in confusing, the work actually increases
- To show: \( B \) is slow
	1. We know \( A \) is slow (by a proof)
	2. Assume \( B \) is quick (toward contradiction)
	3. Show how to use \( B \) properly to perform \( A \)
	4. \( A \) is slow, but \( B \) could be used to perform \( A \) quickly. Conclusion: \( B \) must not actually be quick
- If \( A \) requires time \( \Omega(f(n)) \) time then \( B \) also requires \( \Omega(f(n)) \)
	- \( A ≤_{f(n)}B \): if \( A \) can’t run any faster than \( f(n) \) B can’t run any faster than \( f(n) \)

> [!EXAMPLE] Reducing Element Uniqueness to Closest Pair of Points
> - proves lower bound of \( \Omega(nlogn) \) for CPP
> ![[Screenshot 2025-04-20 at 3.34.59 PM.png]]

## Reductions Conclusions
![[Screenshot 2025-04-20 at 3.19.39 PM.png | center | 400]]

## MaxIndSet \( ≤_V \) MinVertCover

### Maximum Independent Set
- Independent set: \( S \subseteq V \) is an independent set if no two nodes in S share an edge
- Given a graph \( G = (V,E) \), find the maximum independent set \( S \)

### Minimum Vertex Cover
- Vertex Cover: \( C \subseteq V \) is a vertex cover if every edge in \( E \) has one of its endpoints in \( C \)
- Minimum Vertex Cover: Given a graph \( G = (V,E) \), find the minimum vertex cover \( C \)

### Proof: \( \Leftarrow \)
- \( S \) is an independent set of \( G \) iff \( V - S \) is a vertex cover of \( G \)
- Let \( S \) be an independent set
- Consider any edge \( (x,y) \in E \)
- If \( x \in S \), then \( y \notin S \) because otherwise \( S \) would not be an independent set
- Therefore \( y \in V - S \), so edge \( (x,y) \) is covered by \( V-S \)

### Proof: \( \Rightarrow \)
- \( S \) is an independent set of \( G \) iff \( V - S \) is a vertex cover of \( G \)
- Let \( V - S \) be a vertex cover
- Consider any edge \( (x,y) \in E \)
- At least one of \( x \) and \( y \) belong to \( V - S \) because \( V-S \) is a vertex cover of \( G \)
- Therefore \( x \) and \( y \) are not both in \( S \)
- No edge has both end nodes in \( S \), thus \( S \) is an independent set

### Conclusion
- MaxIndSet and MinVertCov are either both fast, or both slow
- Both problems are **NP-Complete**
![[Screenshot 2025-04-20 at 3.55.36 PM.png | center | 500]]