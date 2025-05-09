---
layout: default
title: BFS and DFS
parent: Graphs Notes
nav_order: 3
---
# BFS and DFS

## Breadth First Search

### Specific Input/Output
- **Input**:
	- A graph \( G \)
	- A single start vertex \( s \)
- **Output**:
	- Distance from \( s \) to each node in \( G \) (distance = # of edges)
	- Breadth First Tree of \( G \) with root \( s \)
- **Strategy**
	- Start with node \( s \), visit all neighbors of \( s \), then all neighbors of neighbors of \( s \), …
- **Important**
	- the paths in this BFS tree represent the **shortest paths** from \( s \) to each node in \( G \)
	- status of nodes can be tracked by color:
		- black = node seen, removed from queue, neighbors seen
		- gray = node seen, added to queue for processing
		- white = node not visited yet

![[Screenshot 2025-01-27 at 10.29.43 PM.png | center | 500]]

> [!EXAMPLE] Breadth First Search Pseudocode
> 
> ```
>def bfs(graph, s):
>	toVisit.enqueue(s)
>	mark s as "seen"
>	While toVisit is not empty:
>		current = tovisit.deuque()
>		for v in neighbors(current)
>			if v not seen:
>				mark v as seen
>				toVisit.enqueue(v)

### Shortest Path
- idea: when a node is seen, track its “layer” depth

> [!EXAMPLE] Shortest Path Pseudocode
> ```
> # Idea: when a node is seen, remember its “layer” depth
> def shortest_path(graph, s, t):
> 	layer = 0    # start at layer 0
> 	depth = [-1,-1,-1,...]    # length = # of nodes
> 	toVisit.enqueue(s)
> 	mark a as "seen"
> 	depth[s] = 0    # set the corresponding depth in the array
> 	While toVisit is not empty:
> 		current = toVisit.dequeue()
> 		layer = depth[current]    # set the layer to the depth of the current node
> 		if current == t:    # if the current is the target
> 			return layer    # return the current layer
> 		for v in neighbors(current):
> 			if v not seen:
> 				mark v as seen
> 				toVisit.enqueue(v)
> 				depth[v] = layer + 1    # set the depth to the next layer
> ```

### Tree View of BFS
![[Screenshot 2025-01-28 at 1.50.59 PM.png]]

### Analysis
- For a graph having \( V \) vertices and \( E \) edges
	- Each edge is processed once in the whole loop for the cost of \( \Theta(E) \)
	- Each vertex is put into the queue once and removed from the queue and processed once, for a cost of \( \Theta(V) \)
		- Cost of initializing colors or depth arrays is \( \Theta(V) \)
- Total **time-complexity**: \( \Theta(V+E) \), linear time
- Total **space complexity**:extra space is used for queue and also depth/color arrays/ so \( \Theta(V) \)

## Bipartite Graphs
- An (undirected) graph is **Bipartite** provided every vertex can be assigned to one of two teams such that every edge “crosses” teams
- Every vertex can be given one of two colors such that no edges connect same-color nodes
- A graph is only bipartite if and only if it has no **odd length cycles**
![[Screenshot 2025-05-08 at 8.29.01 PM.png]]

> [!EXAMPLE] BFS Bipartite Pseudocode
> ```
> # Idea: Check for edges in the same layer
> def shortest_path(graph, s, t):
> 	layer = 0 
> 	depth = [-1,-1,-1,...]
> 	toVisit.enqueue(s)
> 	depth[s] = 0
> 	While toVisit is not empty:
> 		current = toVisit.dequeue()
> 		layer = depth[current]
> 		for v in neighbors(current):
> 			if v does not have a depth
> 				depth[v] = layer + 1    # set the depth of the neighboring node
> 				toVisit.enqueue(v)
> 			elif depth[v] == depth[current]:   # if two nodes are neighbors and on the same layer
> 				return False
> 	return True
> ```

### BFS Tree for a Bipartite Graph
![[Screenshot 2025-05-08 at 8.36.26 PM.png]]

### BFS Tree for a Non-Bipartite Graph
![[Screenshot 2025-05-08 at 8.36.49 PM.png]]

## Depth First Search
- Go as deep as possible visiting un-visited nodes
	- Choose any un-visited vertex when you have a choice
- When stuck at a dead-end, backtrack as little as possible
	- Back up to where you could go to another unvisited vertex
- Then continue to go on from that point
- Eventually you’ll return to where you started
	- May or may not have reached all vertices

### Specific Input/Output
- **Input**: a node \( s \)
- **Behavior**: start with node \( s \), visit one neighbor of \( s \), then all nodes reachable from that neighbor of \( s \), then another neighbor of \( s \), …
- **Output**: does the graph have a cycle? A topological sort of graph

> [!EXAMPLE] DFS Non-recursive Pseudocode (less common)
> ```
> def dfs(graph, s):  
> 	toVisit.push(s)  
> 	mark s as “seen”  
> 	While toVisit is not empty:  
> 		current = toVisit.pop()  
> 		for v in neighbors(current):  
> 			if v not seen:  
> 				mark v as seen  
> 				toVisit.push(v)
> ```

> [!EXAMPLE] DFS Recursive Pseudocode
> ```pseudocode
> def dfs(graph, s):
> 	seen = [False, False, False, ...]
> 	done = [False, False, False, ...]
> 	dfs_rec(graph, s, seen, done)
> 
> def dfs_rec(graph, curr, seen, done)
> 	mark curr as seen
> 	for v in neighbors(current):
> 		if v not seen:
> 			dfs_rec(graph, v, seen, done)
> 	mark curr as done
> ```

### Tree View of DFS
- Consider the “seen times” and “done times” of nodes
- As DFS traverses a digraph, edges can be categorized:
	- **Tree Edge**
		- \( (a,b) \) was followed when pushing
		- \( (a,b) \) when \( b \) was unseen when we were at \( a \)
	- **Back Edge**
		- \( (a,b) \) goes to an “ancestor”
		- \( a \) and \( b \) seen but not done when we saw \( (a,b) \)
		- \( t_{seen}(b)<t_{seen}(a)<t_{done}(a)<t_{done}(b) \)
	- **Forward Edge**
		- \( (a,b) \) goes to a “descendant”
		- \( b \) was seen and done between when \( a \) was seen and done
		- \( t_{seen}(a)<t_{seen}(b)<t_{done}(b)<t_{done}(a) \)
	- **Cross Edge**
		- \( (a,b) \) connects “branches” of the tree
		- \( b \) was seen and done before \( a \) was ever seen
		- \( (a,b) \) when \( t_{done}(b)>t_{seen}(a) \)
![[Screenshot 2025-05-08 at 8.41.25 PM.png]]

> [!EXAMPLE] DFS: Cycle Detection Pseudocode
> ```
> # Idea: Look for a back edge
> def dfs(graph, s):
> 	seen = [False, False, False, ...]
> 	done = [False, False, False, ...]
> 	return hasCycle_rec(graph, s, seen, done)    # return result of cycle detection function
> 
> def hasCycle_rec(graph, curr, seen, done)
> 	cycle = False    // initialize cycle to false
> 	mark curr as seen
> 	for v in neighbors(current):
> 		if v seen and v not done:    # if there is a neighbor that has been seen but not done
> 			cycle = True    # there is a cycle
> 		elif v not seen
> 			cycle = dfs_rec(graph, v, seen, done) or cycle    # recurse from not seen neighbors
> 	mark curr as done
> 	return cycle
> ```

### Back Edges in Undirected Graphs
- Finding back edges for an undirected graph is not so simple
	- The parent node of the current node is seen but not done
	- Edges are bidirectional in undirected graphs

> [!EXAMPLE] DFS “Sweep” to Process All Nodes Pseudocode
> ``` pseudocode
> def dfs_sweep(graph):    # no start node given
> 	seen = [False, False, False, ...]
> 	done = [False, False, False, ...]
> 	for s in graph:    # do DFS at every vertex
> 		if s not seen
> 			def_rec(graph, s, seen, done)
> 
> def dfs_rec(graph, curr, seen, done)    # unchanged
> 	mark curr as seen
> 	for v in neighbors(current):
> 		if v not seen:
> 			dfs_rec(graph, v, seen, done)
> 	mark curr as done
> ```

### Time Complexity of DFS
- For a digraph having \( V \) vertices end \( E \) edges
	- Each edge is processed once in the while loop of `dfs_rec()` for a cost of \( \Theta(E) \)
		- Thank about **adjacency list** data structure
		- Traverse each list exactly once (never back up)
		- There are a total of \( E \) modes in all the lists
	- The non-recursive `dfs()` algorithm will do \( \Theta(V) \) work even if there are no edges in the graph
	- Total time-complexity is \( \Theta(V+E) \)
		- Remember: this means the larger of the two values
		- Reminder: this is considered “linear” for graphs since there are two size parameters for graphs
	- Extra space is used for seen/done (or color) array
		- Space complexity is \( \Theta(V) \)