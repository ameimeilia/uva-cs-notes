---
layout: default
title: Introduction
parent: Graphs Notes
nav_order: 2
---
# Introduction

## Graphs
- Definition: $G = (V,E)$ where $V$ = vertices/nodes and $E$ = edges
	- $|V| = n, \ |E| = m$
![[Screenshot 2025-01-21 at 2.25.19 PM.png | center | 500]]

**Directed Graphs**
![[Screenshot 2025-01-21 at 2.26.49 PM.png | center | 500]]

**Weighted Graphs**
- $w(e)$ = weight of edge $e$
![[Screenshot 2025-01-21 at 2.27.31 PM.png | center | 500]]

- **Vocabulary**:
	- **Degree**: number of “neighbors” of a vertex / number of “incident” edges
	- **Indegree**: number of incoming edges
	- **Outdegree**: number of outgoing edges
	- **Relative number of edges to nodes**:
		- What’s the maximum number of edges for an undirected graph? Directed graph?
		- Complete graph
		- Sparse graph vs. dense graph
- **ADT Graph Operations**:
	- Add edge
	- Remove edge
	- Check if edge exists
	- Get neighbors (incoming)
	- Get neighbors (outgoing)
### Data Structures for Graphs
**Undirected Graphs**
![[Screenshot 2025-01-21 at 2.39.56 PM.png | center | 500]]

**Digraphs**
![[Screenshot 2025-01-21 at 2.41.30 PM.png | center | 500]]

**Weighted Graphs**
![[Screenshot 2025-01-21 at 2.42.07 PM.png | center | 600]]

### Operation Costs: Adjacency Matrix
1. Space to represent: $\Theta(n^2)$
2. Add edge $(v,w)$: $\Theta(1)$
3. Remove edge $(v,w)$: $\Theta(1)$
4. Check if edge $(v,w)$ exists: $\Theta(1)$
5. Get neighbors (incoming) of $v$: $\Theta(n)$, check column
6. Get neighbors (outgoing) of $v$: $\Theta(n)$, check row

![[Screenshot 2025-01-21 at 2.54.03 PM.png | center | 200]]
### Operation Costs: Adjacency List
1. Space to represent: $\Theta(n+m)$
2. Add edge $(v,w)$: $\Theta(1)$
3. Remove edge $(v,w)$: $\Theta(deg(v))$
4. Check if edge $(v,w)$ exists: $\Theta(deg(v))$
5. Get neighbors (incoming) of $v$: $\Theta(n+m)$
6. Get neighbors (outgoing) of $v$: $\Theta(deg(v))$

- Note: Vertices may be identified with strings not integers → Could use an **adjacency map**
- Programmers often have an index and/or lookup table to convert between integers and string IDs for vertices

![[Screenshot 2025-01-21 at 2.54.21 PM.png | center | 300]]

### Definitions
- **Path**: a sequence of nodes $(v_1, v_2,…,v_k) s.t. \forall i, 1≤i≤k-1, (v_i,v_{i+1} \in E)$
	- **Simple Path**: a path in which each node appears at most once
	- **Cycle**: a path of > 2 nodes on which $v_1 = v_k$
	- **Acyclic Graph**: has no cycles
	- **Direct Acyclic Graph**: direct graph, no cycles
- **Connected Graph**: a graph $G = (V, E) s.t.$ for any pair of nodes $v_1,v_2 \in V$ there is a path from $v_1$ to $v_2$
	![[Screenshot 2025-01-21 at 3.06.43 PM.png | center | 500]]
	- **Strongly Connected**: path from $v_1$ to $v_2$ is a **directed path**
	- **Weakly Connected**: path from $v_1$ to $v_2$ exists **regardless of directed**
![[Screenshot 2025-01-21 at 3.09.13 PM.png | center | 300]]