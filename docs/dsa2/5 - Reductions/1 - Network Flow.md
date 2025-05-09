---
layout: default
title: Network Flow
parent: Reductions Notes
nav_order: 1
---
# Network Flow
## Flow Networks
![[Screenshot 2025-04-16 at 3.10.03 PM.png | center | 400]]
- Graph $G = (V,E)$
- Start node $s \in V$
- Sink node $t \in V$
- Edge capacities $c(e) \in \mathbb{R}^{+}$

- **Max flow intuition**: if $s$ is a faucet, $t$ is a drain, and $s$ connects to $t$ though a network of pipes $E$ with capacities $c(e)$, what is the *maximum* amount of water which can flow from the faucet to the drain?

## Network Flow
![[Screenshot 2025-04-16 at 3.11.36 PM.png | center | 400]]
- Assignment of values $f(e)$ to edges
	- “Amount of water going through the pipe”
- Capacity contraint
	- $f(x) ≤ c(e)$
	- “Flow cannot exceed capacity”
- Flow constraint
	- $\forall v \in V - {s,t}, \ inflow(v) = outflow(v)$
	- $inflow(v) = \sum_{v \in V}f(x,v)$
	- $outflow(v) = \sum_{v \in V}f(v,x)$
	- “Water going in must match water coming out”
- Flow of $G: |f| = outflow(s) - inflow(s)$
- Net outflow of $s$
	- $outflow(s) = 3$

## Maximum Flow Problem
- Of all valid flows through the graph, find the one that maximizes $|f| = outflow(s) - inflow(s)$
![[Screenshot 2025-04-16 at 3.10.03 PM.png | center | 400]]
- $|f| = 3 + 2 - 1 = 4$

### Greedy Approach
- Saturate highest capacity path first
![[Screenshot 2025-04-16 at 3.19.39 PM.png | center | 400]]
- Observe: highest capacity path is not saturated in optimal solution

## Residual Graphs
- Given a flow $f$ in graph $G$, the residual graph $G_f$ models additional flow that is possible
	- **Forward edge** for each edge in $G$ with weight set to remaining capacity $c(e) - f(e)$
		- Models additional flow that can be sent along the edge: flow to add
	- **Backwards edge** by flipping each edge $e$ in $G$ with weight set to flow $f(e)$
		- Models amount of flow that can be removed from the edge: flow to remove
		- Sometimes need to remove flow from an edge to get the optimal solution
![[Screenshot 2025-04-16 at 5.48.06 PM.png | center | 500]]

![[Screenshot 2025-04-16 at 5.49.24 PM.png | center | 300]]

- Consider a path from $s → t$ in $G_f$ using only edges with positive weight
- Consider the minimum-weight edge along the path: we can increase the flow by $w(e)$
	- Send $w(e)$ flow along all forward edges (these have at least $w(e)$ capacity)
	- Remove $w(e)$ flow along all backward edges (these contain at least $w(e)$ units of flow)
	- Observe: flow has increased by $w(e)$

## Ford-Fulkerson Algorithm
- Define an augmenting path to be an $s → t$ path in the residual graph $G_f$ (using edges of non-zero weight)
- Ford-Fulkerson max-flow algorithm:
	1. Initialize $f(e) = 0$ for all $e \in E$
	2. Construct the residual network $G_f$
	3. While there is an augmenting path $p$ in $G_f$:
		- Let $c = min_{e \in E}c_f(e)$ where $c_f(e)$ is the weight of edge $e$ in the residual network $G_f$
		- Add $c$ units of flow to $G$ based on the augmenting path $p$
		- Update the residual network $G_f$ for the updated flow
![[Screenshot 2025-04-16 at 6.27.25 PM.png | center | 500]]

### Running Time
- Initialization: $O(|E|)$
- Construct residual network: $O(|E|)$
- Finding augmenting path in residual network: $O(|E|)$ using BFS/DFS
- Since we only care about nodes reachable from the source $s$, the number of nodes that are “relevant” is at most $|E|$

- How many iterations are needed?
	- For integer-valued capacities, min-weight of each augmenting path is 1, so number of iterations if bounded by $|f^*|$, where $|f^*|$ is max-flow in $G$ 
	- For rational-valued capacities, can scale to make capacities integer
	- For irrational-valued capacities, algorithm may never terminate!

- Overall runtime for integer capacities: $O(|f^*| \cdot |E|)$
- Highly undesirable if $|f^*| >> |E|$ (e.g. graph is small but capacities are $\approx 2^{32}$)
- Not polynomial-time

### Worst Case Ford-Fulkerson
![[Screenshot 2025-04-16 at 6.40.00 PM.png | center | 400]]
- Observe: each iteration increases flow by 1 unit
- Total number of iterations: $|f^*| = 200$

### Can We Avoid This?
- **Edmonds-Karp Algorithm**: choose augmenting path with fewest hops (find using BFS)
- Running time: $\Theta(|E| \cdot |f^*|, |V| \cdot |E|^2) = O(|V| \cdot |E|^2)$
- From Ford-Fulkerson max-flow algorithm: “While there is an augmenting path in $G-f$, let $p$ be the path with fewest hops”