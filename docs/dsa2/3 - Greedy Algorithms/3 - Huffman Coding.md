# 3 - Huffman Coding

## Introduction: Message Encoding
- Problem: electronically send a message using binary
- Solution: send the message character-by-character with an encoding
- Efficiency:
	- each character requires 4 bits: $l_c = 4$
	- cost of encoding: $B(T, {f_c}) = \sum_{character \ c} l_cf_c = 68 \cdot 4 = 272$

![[Screenshot 2025-03-26 at 3.29.11 PM.png | center | 100]]

- Better solution: allow for different characters to have different-size encodings → high frequency = short code
- when $f_c$ is large, make $l_c$ small

![[Screenshot 2025-03-26 at 12.38.48 PM.png | center | 300]]

- Problem: Morse code has ambiguous decoding

## Prefix-Free Code
- A prefix-free code is a codeword table $T$ such that for any two characters $c_1,c_2$, if $c_1 \neq c_2$ then $code(c_1)$ is not a prefix of $code(c_2)$

### Binary Trees = Prefix-free Codes
- We can represent any prefix-free code as a binary tree
- We can create a prefix-free code from any binary tree
![[Screenshot 2025-03-26 at 12.48.23 PM.png | center | 400]]

### Goal: Shortest Prefix-Free Encoding
- Input: a set of character frequencies $f_c$
- Output: a prefix-free code $T$ which minimizes the cost of encoding $B(T, {f_c}) = \sum_{character \ c} l_cf_c$

## Greedy Algorithms Refresher
- Greedy algorithms require 2 things:
	1. Optimal Substructure
	2. Greedy Choice Function
- Optimal Substructure:
	- if $A$ is an optional solution to a problem, then the components of $A$ are optimal solutions to subproblems
- Greedy Choice Function
	- The rule for how to choose an item guaranteed to be in the optimal solution
- Greedy Algorithm Procedure:
	- Apply the Greedy Choice Function to pick an item
	- Identify your subproblem, then solve it

## Huffman Algorithm
- Choose the least frequent pair, combine into a subtree → subproblem of size $n - 1$

![[Screenshot 2025-03-26 at 3.34.25 PM.png | center | 600]]

### Exchange Argument
- Shows correctness of a greedy algorithm
- Idea:
	- Show exchanging an item from an arbitrary optimal solution with your greedy choice makes the new solution no worse

### Remember: Interval Scheduling Algorithm
- Find event ending earliest, add to solution
- Remove it and all conflicting events
- Repeat until all events removed, return solution

### Remember: Exchange Argument
- Claim: earliest ending interval is always part of some optimal solution

### Showing Huffman is Optimal
- Overview:
	- Show that there is **an** optimal tree in which the least frequent characters are siblings
		- Exchange argument
	- Show that making them siblings and solving the new smaller sub-problem results in **an** optimal solution
		- Optimal Substructure argument

#### Proving Exchange Argument
- First step: show any optimal tree is “full” (each node has either 0 or 2 children)
![[Screenshot 2025-03-26 at 3.51.27 PM.png | center | 400]]

- Claim: if $c_1,c_2$ are the least frequent characters, then there is an optimal prefix-free code such that $c_1,c_2$ are siblings (codes for $c_1,c_2$ are the same length and only differ by their last bit)
	- Case 1: Consider some optimal tree $T_{opt}$. If $c_1, c_2$ are siblings in this tree, then the claim holds
	- Case 2: Consider some optimal tree $T_{opt}$, in which $c_1, c_2$ are not siblings

##### Case 2: $c_1,c_2$ are not siblings in $T_{opt}$
- Let $a,b$ be the two characters of lowest depth that are siblings
- Idea: show that swapping $c_1$ with $a$ does not increase cost of the tree, and similarly for $c_2$ and $b$
- Assume: $f_{c1} ≤ f_a$ and $f_{c2} ≤ f_b$
![[Screenshot 2025-03-26 at 7.56.27 PM.png | center | 400]]

![[Screenshot 2025-03-26 at 7.58.53 PM.png | center | 500]]

#### Proving Optimal Substructure
- Goal: show that is $c_1, c_2$ are siblings in an optimal solution, then an optimal prefix-free code can be found by using a new character $\sigma$ with frequency $f_\sigma = f_{c1}+f_{c2}$ and then making $c_1, c_2$ its children
- By contradiction:
	- Assume that $c_1, c_2$ are siblings in at least one optimal solution
	- **Assume that solving the subproblem with this new character, then adding in $c_1, c_2$ is not optimal**
	- Show that removing $c_1, c_2$ from a better overall solution must produce a better solution to the subproblem
    
##### Show Recursive Substructure
- Show treating $c_1, c_2$ as a new “combined” character gives optimal solution
- Claim: an optimal solution for $F$ involves finding an optimal solution for $F’$, then adding $c_1,c_2$ as children to $\sigma$
![[Screenshot 2025-03-26 at 8.04.26 PM.png | center | 400]]

![[Screenshot 2025-03-26 at 8.05.02 PM.png | center | 500]]

- Suppose $T$ is not optimal, then let $U$ be a lower-cost tree $B(U) < B(T)$
![[Screenshot 2025-03-26 at 8.07.02 PM.png | center | 500]]

![[Screenshot 2025-03-26 at 8.07.29 PM.png | center | 500]]
