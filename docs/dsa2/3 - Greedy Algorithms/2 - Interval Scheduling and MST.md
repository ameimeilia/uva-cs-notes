---
layout: default
title: Interval Scheduling and MST
parent: Greedy Algorithms Notes
nav_order: 2
---
# Interval Scheduling and MST

## Interval Scheduling
- input: list of events with their start and end times
- output: largest set of non-conflicting events
![[Screenshot 2025-03-05 at 7.36.44 PM.png | center | 350]]

1. identify a **greedy choice property**
	![[Screenshot 2025-03-05 at 7.41.44 PM.png | center | 350]]
	1. find event ending earliest, add to solution
	2. remove it and all conflicting events
	3. repeat until all events removed, return solution

> [!example] Interval Scheduling Pseudocode
> ```Python
> sort intervals by finish time
> 
> start_time = 0
> for each interval (in order of finish time):
> 	if beginning of interval > start_time
> 		add interval to solution
> 		start_time = end of interval
> ```

### Exchange Argument
- show exchanging an item from an arbitrary optimal solution with your greedy choice makes the new solution no worse

- **claim**: earliest ending interval is always part of some optimal solution
- let $OPT_{i,j}$ be an optimal solution for time range $[i,j]$
- let $a*$ be the first interval in $[i,j]$ to finish overall
- if $a* \in OPT_{i,j}$ then the claim holds
- else if $a* \notin OPT_{i,j}$, let $a$ be the first interval to end in $OPT_{i,j}$
	- by definition, $a*$ ends before $a$, and therefore does not conflict with any other events in $OPT_{i,j}$
	- therefore, $OPT_{i,j} - {a} + {a*}$ is also an optimal solution
	- thus the claim holds

## Minimum Spanning Trees
### Spanning Tree
- A tree $T = (V_T, E_T)$ is a **spanning tree** for an **undirected** graph $G=(V,E)$ if $V_T = V$, $E_T \subseteq E$
- $T$ connects or “spans” all the nodes in $G$
![[Screenshot 2025-03-23 at 6.13.13 PM.png | center | 400]]
- Each spanning tree has a different total **cost** (sum of edge weights included in tree)
- The **minimum spanning tree** is the spanning tree with lowest overall cost

### Minimum Spanning Tree
- A tree $T = (V_T, E_T)$ is a **minimum spanning tree** for an **undirected** graph $G=(V,E)$ if $T$ is a spanning tree of minimal cost

### MST Algorithms
- Prim’s Algorithm
	- Similar to Dijkstra’s Shortest Path Algorithm
	- Builds a single tree, adding one edge to grow the tree
- Kruskal’s Algorithm
	- In a **forest** of trees, add an edge at each step to grow one tree or to connect two trees (don’t make a cycle)
	- Utilizes an interesting data structure for manipulating sets

#### Prim’s Algorithm
1. Start with an empty tree $T$ and add the source to $T$
2. Repeat $|V| - 1$ times:
	- At each step, add the node with the minimum connecting edge to a node in $T$

**Implementation**:
- Maintain nodes **not in** $T$ in a min-heap (priority queue)
- Find the next closest node $v$ by extracting min from priority queue
- Each time node $v$ is added to the tree, update keys for neighbors still in min-heap
- Repeat until no nodes left in min-heap

> [!example] Prim’s Algorithm Implementation Pseudocode
>```Python
>initialize d_v = infinity for each node v
>add all nodes v in V to the priority queue PQ, using d_v as the key
>pick a starting node s and set d_s = 0
>while PQ is not empty
>	v = PQ.extractMin()
>	for each u in V such that (v,u) in E:
>		if u in PQ and w(v,u) < d_u:
>			PQ.decreaseKey(u,w(v,u))
>			u.parent = v
>```

- Runtime: $O(|V|log|V| + |E|log|V|) = O(|E|log|V|)$

#### Kruskal’s Algorithm
1. Start with an empty set of edges $T$
2. Repeatedly add to $T$ the lowest-weight edge that does not create a cycle (stop when we’ve added $n-1$ edges)

**Implementation**: iterate over each of the edges in the graph (sorted by weight), and maintain nodes in a **union-find** (also called **disjoint-set**) data structure:
- Data structure that tracks elements partitioned into different sets
- **Union**: merges two sets into one
- **Find**: given an element, return the index of the set it belongs to
- Both “union” and “find” operations are very fast

- Runtime: $O(|E|log|E|) = O(|E|log|V|), |E| ≤ |V|^2 \implies log|E| = O(log|V|)$

> [!example] Kruskal’s Algorithm Implementation Pseudocode
>```Python
>Let EL be the set of edges sorted ascending by weight
>Consider each vertex to be in tree of size 1
>For each edge e in EL
>	T1 = tree ID for vertex head(e)
>	T2 = tree ID for vertex tail(e)
>	if (T1 != T2)    # the nodes are not in the same tree
>		Add e to the output set of edges T    # which becomes the MST
>		Combine trees T1 and T2
>```

## Union/Find
### Union/Find and Disjoint Sets
- An Abstract Data Type for a collection of sets of any kind of item, where an item can only belong to one of the sets
	- Assume each item is identified by a unique integer value
- Supports the following operations:
	- `void makeSet(int n)`: construct $n$ independent sets
	- `int findSet(int i)`: given $i$, which set does $i$ belong to?
	- `void union(int i, int j)`: merge sets containing $i$ and $j$

### Represent Sets as Trees
- In our implementation, we’ll represent each set as a tree
- Identify set by its root node’s ID (its “label”)
	- `findSet()` means tracing up to root
	- `union()` makes one root the child of the other root
![[Screenshot 2025-03-23 at 6.42.37 PM.png | center | 400]]

### Operations
###### `void makeSet(int n)`
- Solution: store as an array of size $n$, where each location stores the label for that set
- node # is vertex ID, label is parent node
![[Screenshot 2025-03-23 at 6.44.03 PM.png | center | 350]]

###### `int findSet(int i)`
- Solution: trace around array until we find place where index and contents match
```Python
Start at index i and repeat:
	if a[i] == i then
		return i
	else
		i = a[i]
```

![[Screenshot 2025-03-23 at 6.54.00 PM.png | center | 350]]

###### `void union(int i, int j)`
- Solution: find label for each set (call `find()` method), then set one label to point to the other
```Python
Label1 = find(i)
Label2 = find(j)

a[Label1] = Label2

# OR

a[Label2] = Label1
```