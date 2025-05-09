---
layout: default
title: Max Flow, Min Cut and Bipartite Matching
parent: Reductions Notes
nav_order: 2
---
# Max Flow, Min Cut and Bipartite Matching

## Max Flow/Min Cut

### Reminder: Graph Cuts
- A **cut** of a graph $G = (V,E)$ is a partition of the nodes into two sets, $S$ and $V - S$
- An edge $(v_1,v_2) \in E$ crosses a cut if $v_1 \in S$ and $v_2 \in V - S$
- An edge $(v_1,v_2) \in E$ respects a cut if $v_1,v_2 \in S$ or if $v_1,v_2 \in V - S$

### Correctness of Ford-Fulkerson
- Consider cuts which separate nodes $s$ and $t$
	- Let $s \in S, t \in T$ such that $V = S \cup T$
- Cost of cut $(S,T) = ||S,T||$ (maximum flow that can cross a cut)
	- Sum capacities of edges which go from $S$ to $T$
	- This example: 5
![[Screenshot 2025-04-16 at 7.30.43 PM.png | center | 300]]

### Maxflow ≤ Mincut
- Max flow upper bounded by any cut separating $s$ and $t$
- Why? “Conservation of flow”
	- All flow exiting $s$ must eventually get to $t$
	- To get from $s$ to $t$, all “pipes” must cross the cut
- Conclusion: if we find the minimum-cost cut, we’ve found the max flow
	- $max_f|f| ≤ min_{S,T}||S,T||$

### Maxflow/Mincut Theorem
- To show Ford-Fulkerson is correct:
	- Show that when there are no more augmenting paths, there is a cut with cost equal to the flow
- Conclusion: the maximum flow through a network matches the minimum-cost cut
	- $max_f|f| ≤ min_{S,T}||S,T||$
- Duality
	- When we’ve maximized max flow, we’ve minimized min cut (and vice-versa), so we can check when we’ve found one by finding the other

![[Screenshot 2025-04-16 at 7.37.53 PM.png | center | 400]]

### Proof: Maxflow/Mincut Theorem
- If $|f|$ is a max flow, then $G_f$ has no augmenting path
	- Otherwise, use that augmenting path to “push” more flow
- Define $S =$ nodes reachable from source node $s$ by positive-weight edges in the residual graph
	- $T = V-S$
	- $S$ separates $s,t$ (otherwise there’s an augmenting path)

- To show: $||S,T|| = |f|$
	- Weight of the cut matches the flow across the cut
- Consider edge $(u,v)$ with $u \in S, v \in T$
	- $f(u,v) = c(u,v)$ because otherwise $w(u,v) > 0$ in $G_f$, which would mean $v \in S$
- Consider edge $(y,x)$ with $y \in T, x \in S$
	- $f(y,x) = 0$ because otherwise the back edge $w(y,x) > 0$ in $G_f$, which would mean $y \in S$
![[Screenshot 2025-04-16 at 7.42.56 PM.png | center | 400]]

### Proof Summary
1. The flow $|f|$ of $G$ is upper-bounded by the sum of the capacities of edges crossing any cut separating source $s$ and sink $t$
2. When Ford-Fulkerson terminates, there are no more augmenting paths in $G_f$
3. When there are no more augmenting paths in $G_f$ then we can define a cut $S =$ nodes reachable from source node $s$ by positive-weight edges in the residual graph
4. The sum of edge capacities crossing this cut must match the flow of the graph
5. Therefore this flow is maximal

## Bipartite Matching

### Edge-Disjoint Paths
- Given a graph $G = (V,E)$, a start node $s$ and a destination node $t$, give the maximum number of paths from $s$ to $t$ which share no edges
	- Set of edge-disjoint paths of size 4:
![[Screenshot 2025-04-16 at 8.01.03 PM.png | center | 350]]

- Solve using Edmonds-Karp: make $s$ and $t$ the source and sink, give each edge capacity 1, find the max flow
	- Set of edge-disjoint paths of size 4
	- Max flow = 4
![[Screenshot 2025-04-16 at 8.00.32 PM.png | center | 350]]
![[Screenshot 2025-04-16 at 8.03.10 PM.png | center | 350]]

### Vertex Disjoint Paths
- Given a graph $G = (V,E)$, a start node $s$ and a destination node $t$, give the maximum number of paths from $s$ to $t$ which share no vertices
- Idea: convert an instance of the vertex-disjoint paths problem into an instance of edge-disjoint paths
- Make two copies of each node, one connected to incoming edges, the other to outgoing edges
- Set the capacity of the edge between $g_{in}$ and $g_{out}$ to 1 → even if there are multiple paths going into $g_{in}$, only one can move on to $g_{out}$ 
 - Compute **Edge-Disjoint Paths** on the new graph
![[Screenshot 2025-04-16 at 8.57.11 PM.png | center | 300]]

### Maximum Bipartite Matching
- Biparitite Graph: graph with multiple sets in which no edge connects two elements within one set
- Given a graph $G = (L,R,E)$
	- A set of left nodes, right nodes, and edges between left and right
- Find the largest set of edges $M \subseteq E$ such that each node $u \in L$ or $v \in R$ is incident to at most one edge
- Similar to MST, there can be multiple Maximum Bipartite Matchings
![[Screenshot 2025-04-19 at 8.32.25 PM.png | center | 400]]

### Maximum Bipartite Matching Using Max Flow
- Make $G = (L,R,E)$ a flow network $G = (V,E)$ by:
	- Adding in a source and sink to the set of nodes:
		- $V’ = L \cup R \cup {s,t}$
	- Adding an edge from source to $L$ and from $R$ to sink:
		- $E’ = E \cup{u \in L | (s,u)} \cup {v \in R | (v,t)}$
	- Make each edge cap 1:
		- $\forall e \in E’, c(e) = 1$
![[Screenshot 2025-04-19 at 8.35.23 PM.png]]

### Runtime
- Make $G$ into $G’$: $\Theta(L+R)$
- Compute Max Flow on $G’$: $\Theta(E \cdot V)$ since $|f| ≤ L$
- Return $M$ as “middle” edges with flow 1: $\Theta(L+R)$
- Total: $\Theta(E \cdot V)$