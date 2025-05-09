---
layout: default
title: Introduction to NP-Completeness
parent: Reductions Notes
nav_order: 4
---
# Introduction to NP-Completeness

## Introduction
- What we could prove that some problems are (most likely) hard?
	- “hard” means that it runs in exponential time
	- “easy” means that it runs in polynomial time
- We **want** there to be “hard” problems
	- Cracking encryption should be “hard”
	- But regular encryption and decryption should be “easy”
- Given a new encryption algorithm, how do we show that it’s actually “hard?”

## DMT 1 Review
- Conjunction: AND \( \wedge \)
- Disjunction: OR \( \vee \)
- DeMorgan’s Law:
	- \( \neg (a \vee b) == \neg a \wedge \neg b \)
	- \( \neg(a \wedge b) == \neg a \vee \neg b \)

## Background: Satisfiability (SAT)
- Given a boolean expression, can we find an assignment of true/false values to the variables such that the entire expression is true?
- Examples:
	- \( x_1 \wedge (x_2 \vee \neg x_3) \wedge (\neg x_1 \vee x_3) \wedge (x_2 \vee \neg x_3) \)
		- Yes, \( x_1,x_2,x_3 \) all true
	- \( x_1 \wedge (x_2 \vee \neg x_3) \wedge (\neg x_1 \vee x_3) \wedge (\neg x_2 \vee \neg x_3) \)
		- No, there assignment of true/false that will satisfy this equation
- These equations are in **conjunction normal form**: a conjunction of disjunctions
	- Keep this as a standard format
- Only exponential-time algorithms to solve these problems
- Problems are hard to solve, but easy to verify when given an answer

## Automata
![[Screenshot 2025-04-20 at 4.22.16 PM.png | center | 400]]
- Finite State Machine:
	- Each arrow assumes that it pops (consumes) one character
	- Double circled states are final states
		- If the end of input is in a final state, the string is accepted

### As Code
``` Python
def check_uva_userid2(what):
	# returns True if the passed parameter is a valid UVA userid, else False
	
	state_table = [4 
		# from each state, where to go on an 'l', 'd', and 'o', respectively
		[], # we numbered our states from 1, so we burn spot 0
		[2, None, None], # state 1 goes to 2 on a letter
		[3, None, None], # state 2 goes to 3 on a letter
		[4, 5, None], # state 3 goes to 4 on a letter and 5 on a digit
		[None, 5, None], # state 4 goes to 5 on a digit
		[6, None, None], # state 5 goes to 6 on a letter  
		[7, None, None], # state 6 goes to 7 on a letter  
		[8, None, None], # state 7 goes to 8 on a letter  
		[None, None, None], # no transitions out of state 8
	]
	final_states = [3, 4, 6, 7, 8]
	
	chars = list(what.lower())
	state = 1
	while len(chars) > 0:

		# which is whether it's a letter (0), digit (1), or other (2)
		which = 0 if chars[0].isalpha() else 1 if chars[0].isdigit() else 2

		# get the state transition, and verify it's not None
		next_state = state_table[state][which]
		if next_state is None: return False

		# transition to that state and pop the input character
		state = next_state
		chars.pop(0)

	# we should have ended in a final state
	return state in final_states
```

### Non-determinism
- What if we only have one accepting (final) state?
- ex. In state 2, on an input of a letter, we could go to two different states: 3 or 8
![[Screenshot 2025-04-20 at 4.35.44 PM.png | center | 400]]

![[Screenshot 2025-04-20 at 4.37.03 PM.png | center | 250]]

- Non-determinism makes it easier to model many “things”
	- ex. Regular expression
- All non-deterministic finite automata (NFAs) can be converted into deterministic finite automata (DFAs)
	- NFAs have to be converted for computers (deterministic) computers to use them
- Sometimes converting an polynomial-time NFA yields an exponential time DFA

- empty (\( \epsilon \)) transitions: transition between states without consuming a character
![[Screenshot 2025-04-20 at 5.42.02 PM.png | center | 250]]

### Automata Definitions
- DFA: Deterministic Finite Automata
	- From any state, on any input, there is at most one destination state
- NFA: Non-deterministic Finite Automata
	- There is at least one state, which on some input, has more than one possible destination state
	- and/or there is at least one empty transition
- The range of languages (strings) that these can produce (accept) are called **regular** languages

### Automata and Languages
- How would you write an automata that can accept strings with **some** positive number of a’s followed by some positive number of b’s?
	- Expressed as \( a^+b^+ \)
![[Screenshot 2025-04-20 at 5.50.54 PM.png | center | 100]]

- How would you write an automata that can accept strings with some positive number of a’s followed by **the same number** of b’s?
	- Expressed as \( a^nb^n \)
- Impossible, would need a infinite automata since a finite automata of \( n \) states cannot accepts strings of \( n+k \) a’s
![[Screenshot 2025-04-20 at 6.31.02 PM.png | center | 100]]

### Accepting \( a^n b^n \)
- To accept these strings, you need **memory**: how many a’s have been accepted
- This is done via a stack
	- On each a, push a value onto the stack
	- On each b, pop a value from the stack
- Ensure the stack is empty at the end
- This is called a **push-down automata**
- The range of languages (strings) that these can produce (accept) are called **context-free** languages

### Even More Powerful Automata
- What about \( a^n b^n c^n \)?
- Need a **linear-bounded automata**
	- An automata with a limited form of memory
- The range of languages (strings) that these can produce (accept) are called **context-sensitive** languages

### Turing Machines
- Even more powerful Automata
- Can compute anything a computer can compute
	- Such languages are called **recursively enumerable** languages
- A Turing machine has a NFA (or DFA) as it’s “control”
	- A computer’s current memory state is, in effect, a DFA state
- And an infinite tape that it can ready and write values to
	- Analogous to a computer’s memory
![[Screenshot 2025-04-20 at 6.37.13 PM.png | center | 250]]

### NFA Animation
- Imagine that this is the NFA in a Turing machine
	- This one does not use memory
- Trace through accepting any valid UVA userids
![[Screenshot 2025-04-20 at 7.03.28 PM.png | center | 400]]![[Screenshot 2025-04-20 at 7.03.58 PM.png | center | 400]]
![[Screenshot 2025-04-20 at 7.04.24 PM.png | center | 400]]

## Cook-Levin Theorem (1971)
- Any such problem (of the type we have seen) that can be solved by a Turing machine (and thus an automata) can be reduced to satisfiability