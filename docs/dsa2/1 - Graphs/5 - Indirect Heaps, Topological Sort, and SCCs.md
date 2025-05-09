---
layout: default
title: Indirect Heaps, Topological Sort, and SCCs
parent: Graphs Notes
nav_order: 5
---
# Indirect Heaps, Topological Sort, and SCCs

## Indirect Heaps
- Indirect heaps are an example of the common computing principle of **indirection**:
	- Simple example: an implementation of `FindMax(anArray)` that returns the array index of the maximum value instead of the value itself
	- Other examples: pointers in C/C++, object references in Java and Python
- Idea: have some kind of “index” that, given a node’s “ID,” you can quickly find where that node is in the heap’s tree
![[Screenshot 2025-05-08 at 9.15.34 PM.png | center | 500]]

## Topological Sort
- A topological sort of a **directed acyclic graph** $G = (V,E)$ is a permutation of V such that if $(u,v) \in E$ then $u$ is before $v$ in the permutation
- Idea: list in reverse order by finish time
![[Screenshot 2025-02-17 at 11.09.16 AM.png | center | 400]]

> [!example] Topological Sort Pseudocode
> ```Python
> def top_sort(graph): # has loop like dfs_sweep
> 	seen = [False, False, False, ...] # length matches |V|
> 	finished = []
> 	for s in graph.V
> 		if s not seen:
> 			finish_time(graph, s, seen, finished)
> 		return reverse(finished)
> 
> def finish_time(graph, curr, seen, finished):
> 	seen[curr] = True
> 	for v in neighbors(current):
> 		if v not seen:
> 			finish_time(graph, v, seen, finished)
> 	finished.append(curr)
> ```

## Strongly Connected Components (SCCs)
- In a digraph, Strongly Connected Components are subgraphs where all vertices in each SCC are reachable from one another
	- Thus vertices in an SCC are on a directed cycle
	- Any vertex not on a directed cycle is an SCC all by itself
- Common need: decompose a digraph into its SCCs
	- Perhaps then operate on each, combine results based on connections between SCCs

### SCC Example
- Digraph below has 3 SCCs
	- Each SCC has a cycle (possible to have a single node SCC)
	- There are connections to other SCCs, but no path leaves a SCC and comes back
	- There is a unique set of SCCs for a given digraph
![[Screenshot 2025-05-08 at 9.18.49 PM.png | center | 500]]

### Decomposing a Digraph into SCCs
1. Call `dfs_sweep(G)` to find finishing times $u.f$ for each vertex $u$ in $G$
2. Compute $G^T$, the transpose of digraph $G$ (same nodes, edges reversed)
3. Call `dfs_sweep(G^T)` but do the recursive calls on nodes in the oder of decreasing $u.f$ from Step 1 (start with the vertex with the largest finishing time in $G$’s DFS tree)
4. The DFS forest produced in Step 3 is the set of SCCs
![[Screenshot 2025-05-08 at 9.25.15 PM.png | center | 500]]

### Transpose and Finish Times
- A digraph and its transpose have the same SCCs
	- Use the fact that edge-directions are reversed in $G^T$ to stope DFS from leaving an SCC
- In $G^T$, find SCCs in reverse order of finish time
	- Essentially a topological sort for $G^{SCC}$
