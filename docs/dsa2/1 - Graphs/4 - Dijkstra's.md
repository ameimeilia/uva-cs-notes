# 4 - Dijkstra's

## Shortest Path
### Dijkstra’s Algorithm
- Input: graph with no negative edge weights, start node $s$, end node $t$
- Behavior: start with node $s$, repeatedly go to the incomplete node “nearest” to $s$, stop when
- Output: distance from start to end, distance from start to every node
- Procedure:
	1. Start with an empty tree $S$ and add the source to $S$
	2. Repeat $|V| - 1$ times:
		- At each step, add the node “nearest” to the source not yet in $S$ to $S$

### Data Structure to Store Nodes
- Strategy: at every step, choose node not in $S$ that’s closest to source
- To do this efficiently, we need a data structure that:
	- Stores a set of (node, distance) pairs
	- Allows efficient removal of the pair with smallest distance
	- Allows efficient additions and updates
- Use Priority Queue ADT and binary min-heap (node with smallest priority at the root)

### Review: Heap
- Store the elements in a **one-dimensional array** in strict left-to-right, **level order**
- Store the key(priority) value
- Usually ignore index 0
- Formulas:
	- Left child: $2i$
	- Right child: $2i+1$
	- Parent: $floor(1/2)$
![[Screenshot 2025-02-03 at 10.32.40 PM.png | center | 400]]

#### Heap Operations
##### `extractMin()`
- Returns and removes the item with the min key (e.g. the heap’s root)
- Move the last item to root and “bubble it down” to the correct location
- Complexity: $O(logn)$
##### `insert(item, key)`
- Add a new item at the end of the array and “bubble it up” to the correct location
- Complexity: $O(logn)$
##### `decreaseKey(item, newKey)`
- Find item in min-heap, decrease its key, and “bubble it up” to the correct location
- Complexity:
	- Sequential search: $O(n)$
	- Indirect heaps: $O(logn)$

## Dijkstra’s Algorithm
1. Start with an empty tree $S$ and add the source to $S$
2. Repeat $|V| - 1$ times:
	- At each step, add the node “nearest” to the source not yet in $S$ to $S$

> [!example] Implementation Pseudocode
> ```
> initialize depth = infinity for each node v
> add all nodes v in V to the priority queue PQ, using depth as the key
> set depth of source = 0
> while PQ is not empty:
> 	v = PQ.extractMin()
> 	for each u in V s.t. (v,u) in E:
> 		if u in PQ and depth + w(v,u) < depth of u:
> 			PQ.decreaseKey(u,depth + w(v,u))
> 			u.parent = v
> ```

- Observe: shortest paths from a source forms a **tree**
- **Optimal substructure property**: every subpath of a shortest path is itself a shortest path
- Idea: when a node is the closest undiscovered thing to the start, we have found its shortest path
- Complexity:
	- Initialization: $O(|V|)$
	- While loop: $|V|$
	- Extraction: happens once per vertex in the while loop, $O(log|V|)$
	- Decreases: happens at most 1 time to every edge, $O(ElogV)$
	- Overall: $O(|V|log|V| + |E|log|V|=O(|E|log|V|)$ or $O(mlogn)$

> [!example] Python-like Implementation
> ```Python
> def Dijkstras(graph, start, end):
> 	distances = [infinity, infinity, infinity, ...] # one index per node
> 	done = [False, False, False, ...] # one index per node
> 	PQ = priority queue # e.g. a min heap
> 	PQ.insert((0, start))
> 	distances[start] = 0
> 	while PQ is not empty:
> 		current = PQ.extractmin()
> 		if done[current]: continue
> 		for each neighbor of current:
> 			if not done [neighbor]:
> 				new_dist = distances[current] + weight(current, neighbor)
> 				if new_dist < distances[neightbor]:
> 					distances[neighbor] = new_dist
> 					PQ.insert((new_dist, neighbor))
> 	return distances[end]
> ```

### Dijkstra’s Algorithm Proof Strategy
- **Proof Idea**: we will show that when node $u$ is removed from the priority queue, $d_u = \delta(s,u)$ where $\delta (s,u)$ is the shortest distance
	- **Claim 1**: There is a path of length $d_u$ (as long as $d_u < \infty$) from $s$ to $u$ in $G$
	- **Claim 2**: For every path $(s, …, u), w(s, …, u) ≥ d_u$
- **Inductive Hypothesis**: suppose that nodes $v_1 = s, …, v_i$ have been removed from PQ, and for each of them $d_{v_i} = \delta(s,v_i)$, and there is a path from 4s$ to $v_i$ with distance $d_{v_i}$ (whenever $d_{v_i} < \infty$)
	- **Base case**: $i=0: v_1 = s$, claim holds trivially
- **Definitions**:
	- A **cut** of a graph $G = (V, E)$ is a partition of the nodes into two sets, $S$ and $V - S$
	- An edge $(v_1, v_2) \in E$ crosses a cut if $v_1 \in S$ and $v_2 \in V - S$

#### Correctness of Dijkstra’s Algorithm: Claim 1
- Let $u$ be the ($i + 1$)th node extracted
- **Claim 1**: There is a path of length $d_u$ (as long as $d_u < \infty$) from $s$ to $u$ in $G$
- **Proof**:
	1. Suppose $d_u < \infty$
	2. This means that PQ.decreaseKey was invoked on node $u$ on an earlier iteration
	3. Consider the last time PQ.decreaseKey is invoked on node $u$
	4. PQ.decreaseKey is only invoked when there exists an edge$(v,u) \in E$ and node $v$ was extracted from PQ in a previous iteration
	5. In this case, $d_u = d_v + w(v,u)$
	6. By the inductive hypothesis, there is a path $s → v$ of length $d_v$ in $G$ and since there is an edge $(v,u) \in E$, there is a path $s → u$ of length $d_u$ in $G$

#### Correctness of Dijkstra’s Algorithm: Claim 2
- Let $u$ be the ($i + 1$)th node extracted
- **Claim 2**: For every path $(s, …, u), w(s, …, u) ≥ d_u$
	- Extracted nodes “cuts” G into two subsets, $(S, V-S)$
	- Take any path $(s, …, u)$
	- Since $u \notin S, (s, …, u)$ crosses the cut somewhere
	- Let $(x,y)$ be the last edge in the path that crosses the cut
	- Inductive hypothesis: since $x$ was extracted before, $d_x = \delta (s,x)$
		- $w(s, …, u) ≥ \delta (s,x) + w(x,y) + w(y, …, u)$
		- $w(s, …, u) = d_x + w(x,y) + w(y, …, u)$
	- Dijkstra’s: when $x$ is extracted, $d_y$ is updated to satisfy $d_y ≤ d_x + w(x,y)$
		- $w(s, …, x) ≥ d_y + w(y, ..., u)$ 
	- Greedy choice property: we always extract the node of minimal distance so $d_u ≤ d_y$
		- $w(s, …, x) ≥ d_u + w(y, ..., u)$ 
	- All edge weights assumed to be positive
		- $w(s, …, x) ≥ d_u$
        
#### Correctness of Dijkstra’s Algorithm
- **Conclusion**: all paths $(s, …, u)$ are no shorter than $d_u$ which makes it the shortest path (or one of equally shortest paths)