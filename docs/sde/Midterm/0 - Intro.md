---
layout: default
title: Intro
parent: Midterm Notes
nav_order: 1
---
# Intro
### Scale
- “A master craftsperson must know how to use all kinds of tools, and must constantly learn the usage of new tools.” !!!!
- **enterprise software**: large software systems built by teams that respond to change over time
- “small scale ad hoc problem-solving is **fundamentally different** from building larger, sustainable, systematic solutions to problems
### Failures
- “Software fails when the defect _isn’t found_.”
### Complexity
- “software is _essentially_ complex”
- **essential difficulties**: intrinsic to developing software
- **accidental difficulties**: emerge due to circumstance
- essential difficulties will always exist despite improvements to reduce accidental difficulties
#### No Silver Bullet
##### Introduction
- “There is no single development, in either technology or management technique, which by itself promises even one order-of-magnitude improvement within a decade in productivity, in reliability, in simplicity.”
##### Does It Have To Be Hard? - Essential Difficulties
- specification, design, and testing of the conceptual construct is the essence of software
- inherent properties of software essence:
	1. **complexity**
		- software is complex because no two parts are alike
		- scaling-up software requires an increase in the number of different elements
		- elements interact in nonlinear fashion → complexity increases nonlinearly
		- results in both technical and management problems
	2. **conformity**
		- software must conform to human institutions and systems
		- changes depending on time period and person responsible for design
	3. **changeability**
		- pressures to change: software embodies function, software is “easy” to physically change
		- users and new machine push limits of existing software
	4. **invisibility**
		- software is inherently un-visualizable
		- impedes design and communication process
##### Past Breakthroughs Solved Accidental Difficulties
- major three steps in software technology:
	1. **high level languages**
		- embodies the conceptual construct while avoiding machine-level concerns
	2. **time-sharing**
		- much more efficient compared to batch processing
	3. **unified programming environments**
		- allowed conceptual structures to easily call, feed, and use one another
##### Hopes for the Silver
1. **Ada and other high-level language advances**
	- modern software design techniques: modularization, ADT, hierarchical structures
2. **OOP**
	- ADT: types defined by name, values, and operations, rather than storage structure
	- hierarchical types: general interfaces + subordinate types
3. **AI**
	- “most of the work is problem-specific, and some abstraction or creativity is required to see how to transfer it”
4. **expert systems**
	- uses a generalized inference engine + rule base + input data + assumptions = conclusions and advice
	- application independent, can be applied to different uses
	- changes occur in the rule base → regularizes complexity
	- makes accumulated wisdom of the best programmers available to inexperienced ones
5. **”automatic” programming**
	- “program for solving a problem from a statement of the problem specifications”
	- solution method must be provided
6. **graphical programming**
	- difficult to obtain a global overview of a software system
7. **program verification**
	- perfect program verification only ensures a program meets its specifications
8. **environments and tools**
	- biggest problems have already been solved
9. **workstations**
	- cannot speed up the thinking process
##### Promising Attacks on the Conceptual Essence
1. **buy vs. build**
	- buying from software mass market instead of building from new
2. **requirements refinement and rapid prototyping**
	- detailed technical requirements are the hardest to establish and change
	- approaches and tools for iterative prototyping
3. **incremental development - grow, not build, software**
	- top-down growing, start with running system then develop subprograms
4. **great designers**
	- identify great designers and nurture their growth
### Improving
- “It’s the **struggle** that makes you stronger.”
- “you can never truly succeed if you don’t give yourself a significant chance to fail, and rise above it”
### Software Quality
- ISO 9126: internal standard for the evaluation of *software quality*
#### External Quality
- quality of software from the **stakeholder** perspective
- **stakeholders**: anyone affected by the software
- external quality measures:
	1. **functionality**: the extent at which the system meets the customer’s needs
	2. **reliability**: the capability of the software to maintain performance under certain conditions over a certain period of time
	3. **usability**: the amount of effort needed for the customer to use the software
	4. **efficiency**: the resources and extent they are used while operating
	5. **portability**: ability for the software to be transferred from one environment to another
#### Internal Quality
- quality of software from the **developer** perspective
- software **maintenance** achieved through **design**:
	1. **analyzability**: extent to which the software construction can be understood
		- write clean code, use decomposition to make code modular
	2. **changeability**: effort required to change the software
		- use version control and good design principles
	3. **stability**: extent to which changes to one part of the software affect other parts
		- use modularity and design patterns to decouple unrelated portions
	4. **reusability**: extent to which parts of the construction can be reused
		- use OOP and refactoring
	5. **testability**: extent to which the software can be tested to find bugs
		- use JUnit and Test Driven Development
	- **software entropy**: maintainability of software decreases over time, as software becomes larger and more complex