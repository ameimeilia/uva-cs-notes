# 5 - NP-Completeness

## Complexity Classes
- **P**: Any problem that can solved by a **deterministic** finite automata (DFA) in polynomial time
	- Solution can be **verified** polynomial time
- **NP**: Any problem that can solved by a **non-deterministic** finite automata (NFA) in polynomial
	- Might take exponential time on a DFA
	- Solution can be **verified** in polynomial time
- If a problem can be solved by a DFA in polynomial time, then it can be solved by an NFA in polynomial
	- Thus, $P \subset NP$
- **NP-hard**: A problem that is at **least** as difficult as NP
	- There are believed to be problems in NP that are not NP-hard (none yet proven, called NP-Intermediate)
- **NP-complete**: A set of problems that are equivalent difficultly as SAT
	- We show problem $X$ is **NP-complete** by:
		- Reducing SAT (or other **NP-complete** problem) to $X$
		- Reducing $X$ to SAT (or other **NP-complete** problem)
			- But if $X \in NP$, then it reduces to SAT via the Cook-Levin Theorem
			- So we just have to show $X \in NP$
![[Screenshot 2025-04-21 at 12.02.20 AM.png | center | 200]]

## Recap: Satisfiability (SAT)
- No efficient solution to SAT has been found yet
- We know it’s in **NP**
	- As we can verify a solution in polynomial time
- What if we want to show that some problem $X$ is just as difficult as SAT?
- To show **NP-completeness** show that both:
	- $X$ reduces to SAT: $X ≤_p SAT$
	- SAT reduces to $X$: $SAT ≤_p X$
- That would mean they are (roughly) equivalent in difficulty
	- And that likely no efficient solution can be found

## Showing $X \in NP$
- Via a proof not shown, it has been shown that:
	- A problem $X$ being in **NP**
		- Meaning a solvable by a NFA in polynomial time
	- And a solution to $X$ being able to be verified in polynomial time
	- Are actually the same thing
- So we just have to show that we can verify a problem’s solution in polynomial time to show it is in **NP**

### Example Reduction: Vertex Cover
- Consider the minimum vertex cover problem
	- Given a graph $g = (V, E)$, find a set of vertices $C \subseteq V$ such that every edge in $E$ has at least one endpoint in $C$
- To prove it’s **NP-complete**
	- Show it’s in **NP** (this means it reduces to SAT)
		- $VC \in NP \equiv VC ≤_p SAT$
		- Given a set of vertices $C$, we can, in polynomial time, verify all edges in $E$ have at least one endpoint in $C$
			- We just check each edge has at least one endpoint in $C$ which takes $\Theta(E)$ time
	- Reduce a known **NP-complete** problem to it
		- $SAT ≤_p VC$
		- Maximum Independent Set is **NP-complete**
		- So we reduce Maximum Independent Set to Minimum Vertex Cover
	- Thus, the vertex cover problem is **NP-complete**

## Counter-intuitiveness
- Two things tend to be counter-intuitive when trying to prove that problem $X$ is **NP-complete**
	1. You are not *solving* the problem, or giving a direct algorithm to solve it
		- You are only showing that it reduces to and from another **NP-complete** problem (for which you may not know a algorithm for)
	2. You reduce another **NP-complete** problem *to* problem $X$
		- Recall that we want to show both:
			- $X ≤_p SAT$ (or another **NP-complete** instead of SAT)
			- $SAT ≤_p X$ (or another **NP-complete** instead of SAT)
		- But this first one is already proved (assuming you show it’s in **NP**)
		- So you only have to show the second of those ($SAT ≤_p X$)

## Why we can’t just reduce $X$ to SAT
- Consider the problem of finding *a* bipartite matching (not necessarily maximal)
- Reduce it to SAT:
	- Create a set of clauses for the possible matchings
	- OR all these clauses together
	- Negate, via DeMorgan’s Law, into conjunctive normal form
- Solve using SAT
- Solution: DeMorgan-ize it again out of conjunctive normal form, and examine the clauses with the correct pairings

- What did we just show?
	- Non-maximal bipartite matching can be reduced to SAT
	- But non-maximal bipartite matching is much easier than SAT
		- A non-maximal matching can be done in $O(V)$ time: go through the left nodes, and assign them to an arbitrary right node that is not yet assigned
	- This doesn’t show that non-maximal bipartite matching is as difficult as SAT
		- Only that SAT is more difficult than non-maximal bipartite matching

## Can SAT be solved in polynomial time?
- Nobody has found an efficient solution
	- But it doesn’t mean one doesn’t exist
- It has not yet been proved that one can *not* solve it in polynomial time
	- Just that nobody has yet figured out a way to do so
- If it can’t be solved in polynomial time, then $P \neq NP$
- If it *can* be solved in polynomial time, then $P = NP$
	- This would be *bad*, all modern encryption would be crackable
![[Screenshot 2025-04-27 at 4.28.46 PM.png | center | 350]]