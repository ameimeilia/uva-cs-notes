---
layout: default
title: Design
parent: Midterm Notes
nav_order: 9
---
# Design
### What is Design in Software:
- **Design** refers to organizing code, including classes, functions, and their relationships.
- Every software has a design; the key is whether that design is good.
### Before Diving Into Design:
- **Challenges of Design**:
    1. Design flaws often revealed by changes.
    2. No perfect design; all designs have limitations.
    3. Over-design can make software fragmented and difficult to navigate.
    4. Design needs are subjective to the problem.
    5. Measuring design quality is subjective.
- **Purpose of Design**:
    - Focus on design principles to meet customer needs, not perfection.
### What Makes a Good Design?
- No universal answer for "good" design in software.
- In physical systems (e.g., bridges, cars), good design is judged by how long it lasts with maintenance.
- Software differs because it is **malleable** and expected to change.
### Software Changes:
- Software is designed to evolve to meet new needs, unlike physical systems.
- Changes in software can introduce more bugs, making changes difficult.
### Software Entropy:
- As features are added, software becomes more complicated and harder to maintain.
- Good design slows the growth of **software entropy**, making changes easier and cost-effective.
- **Design quality**: measured by the effort required to meet customer needs.
### Planning for Change:
- Expect and plan for changes, even if the exact changes are unknown.
- **Compartmentalization**: helps minimize the impact of changes on the overall system.
### A Note on Over-Design:
- Avoid **over-design** for changes that may never happen.
- Example: Small, short-lived software may not need over-design, whereas long-term, safety-critical systems may need more robustness.
### Agile Design:
- Design decisions should reflect the software's expected lifespan and adaptability needs.
- **Agile development**: allows for iterative improvement and refactoring, accommodating unforeseen changes.
- **Modular design**: enables easier evolution and change during the software lifecycle.
# Concepts
### Modularity
- Breaks big problems into smaller, manageable parts (modules).
- A module can be a class in OO languages (Java) or a function/group of functions in non-OO languages (C).
- Supports decomposition: breaking the system hierarchically into smaller, easier-to-solve units.
- Key focus: cohesive modules (focus on one feature) and loose coupling (simple interactions between modules).
### Functional Independence
- Modules perform their purpose with minimal interaction with other parts of the system.
- Modules should interact through simple mechanisms like passing arguments or returning values.
- Aim for low coupling to avoid unintended side effects when changes occur.
- Tight coupling makes it difficult to change/replace modules later.
### Abstraction
- Hides unnecessary details and exposes only essential features through a minimal interface.
- Keeps implementation details hidden behind the interface (e.g., Java's `ArrayList` hides the underlying array).
- Changes to implementation should not affect other parts of the system if the interface remains unchanged.
### Information Hiding
- Mechanism for enforcing abstraction by limiting what is exposed to users of a module.
- Typically achieved with private fields and public methods (e.g., getters/setters) to control access.
- Prevents clients from directly accessing internal data, ensuring the module is used correctly.
- Some languages (e.g., Python) don’t enforce strict information hiding.
### Trade-offs
- Every design decision involves trade-offs between flexibility, complexity, and usability.
- The goal is to balance design principles (modularity, independence, abstraction) while accounting for system needs.
# Modularity
- **Modularity** refers to breaking down a large problem into smaller, manageable problems. Two types of decomposition are used:
    - Functional decomposition
    - Class decomposition
### Functional Decomposition
- **Functional decomposition** involves breaking a complex process into smaller functions.
- Advantages:
    - Easier to break up work among developers.
    - Encourages reuse of functions (e.g., using the same method for quotient calculation in different steps).
- **Limitations**:
    - Assumes a sequential system, which doesn’t account for asynchronous or persistent systems.
    - High-level functions depend on low-level ones, making changes harder to manage in complex systems.
### Class Decomposition
- **Class decomposition** breaks the system into classes, each responsible for a specific behavior.
- Cohesive data classes focus on storing related data, while other classes handle more functional behaviors.
### Cohesiveness
- Refers to how focused a class or module is on a specific purpose.
- A highly cohesive class has parts that are interdependent and logically related.

**Levels of Cohesion (from Worst to Best)**

1. **Coincidental Cohesion (Worst)**:
    - Elements are grouped randomly with no logical connection.
    - Makes code hard to reuse and maintain (e.g., a large `Main` class).
2. **Logical Cohesion**:
    - Elements share a broad activity but are otherwise unrelated.
    - Example: A class reading state data from different sources (CSV, JSON, database).
    - Better to separate each source type into its own module.
3. **Temporal Cohesion**:
    - Elements are grouped because they occur at the same time (e.g., initialization routines).
    - These should be separated into individual modules.
4. **Procedural Cohesion**:
    - Activities are grouped to execute in a specific order, but they might not all contribute to the same functional purpose.
    - Example: A `RegisterAction` class handling multiple steps of course registration.
5. **Communicational Cohesion** (Decent):
    - Methods are grouped because they operate on the same data (e.g., a `StudentRecord` class managing grades and GPA).
    - May lead to large, hard-to-maintain classes.
6. **Sequential Cohesion (Best)**:
    - Elements must occur in a sequence where the output of one step is the input to the next.
    - Example: A `DeansListPrintout` class where each method depends on the output of the previous one.
7. **Functional Cohesion (Best)**:
    - A class or function focuses entirely on a single, well-defined task.
    - Highest level of cohesion, ensuring clarity and reusability.
# Functional Independence
- When decomposing systems into modules, interactions between them should be as simple as possible.
- Applies to both functional decomposition and class decomposition.
## Coupling
- **Coupling**: Degree of interaction between two modules.
    - Differs from **cohesion**, which refers to dependencies within a module (high cohesion is good, intra-dependency).
    - Loose coupling (low interdependency) is desirable.

**Types of Coupling WORST to BEST**
1. **Content coupling**
	- One class modifies the private state of another class directly (without using public interfaces)
	- Solution: Encapsulate interaction in one class
1. **Common coupling**
	- Modules have **read and write** access to the same global data.
	- Solution: Replace global variables with method arguments
1. **Control coupling**
	- Occurs when one module passes control information (like booleans or enums) to another.
	- Boolean arguments also reduce understandability
	- Solution: Replace with specific getters
1. **Stamp coupling**
	- Occurs when a module passes a large data structure, but only part of it is used.
	- Solution: Pass only the necessary data
1. **Data coupling**
	- Pass only the necessary data, and return only the required result.
	- Advantages:
	    - No side effects or unintended interactions between modules.
	    - Modules can be developed independently without concern for the internal workings of others.
### Mutability and Coupling
- **Mutability issues** arise when a method modifies an object passed between modules.
- **Trade-off**:
    - Returning a new object improves understandability but increases memory usage.
    - Balancing maintainability with performance is often necessary.
### Temporal Coupling
- **Temporal coupling** occurs when methods must be called in a specific order.
- Solution: Use a constructor to eliminate the need for sequential calls.
# Abstraction
- **Abstraction principle**: All behavior related to a particular module should be handled through that module's interface.
### General guideline for abstraction:
- A module should interact with another module only through the interface of that module.
- Example:
    - If Module A uses Module B, and Module B uses Module C, Module A should not interact directly with Module C.
### Using Information Hiding
- Abstraction goes beyond `public`, `private`, and `protected`.
- **Fields should be `private`**:
    - Better to use getters and setters for fields to:
        - Prevent illegal `set` calls.
        - Return safe copies of mutable fields using methods like `clone()`.
- **Direct field access** should be avoided:
    - Prevents any restrictions or control over how fields are accessed or modified.
- **Changing implementation**:
    - Fields can be modified (e.g., replacing a `List` with a `Set`) without affecting the public interface.
- Only expose methods that need to be publicly available.
- **Helper methods**:
    - Should be private or package-private, not public, as they are internal to the class.
### Java Visibility Keywords
- `public`: Accessible by any class.
- `protected`:
    - Accessible by any class **in the same package**.
    - Also accessible by subclasses, even in different packages.
# Design Principles
## DRY Principle
- **Don’t Repeat Yourself**
- use functions or polymorphism when we are sharing the same knowledge.
- BUT: never unnecessarily sacrifice **understandability** and **functional independence** for DRY-ness.
### The Abstraction/Coupling Trade-off
- **Abstraction creates coupling**
#### Solution:
- **Avoid over-complicating abstractions**:
    - Use **Aggregation** (class contains another class) instead of **Inheritance** (class is a subtype of another).
### YAGNI Principle
- **YAGNI**: "You ain't gonna need it."
- **Avoid future-proofing**: Only add features when necessary.
- **Plan-driven vs Agile**: Overdesign in plan-driven wastes time. Agile emphasizes adding features as needed and refactoring.
- **Risk**: Overdesign makes code harder to understand, even if it theoretically improves changeability.
# Class Relationships
## Dependency
- Occurs when one class uses methods or functions from another.
- Key takeaway: More method interactions between classes mean tighter coupling, making changes more complex.
## Aggregation
- Occurs when a class "has" instances of other classes as fields.
- Aggregation is crucial to describe an object’s state but should still aim for data-level coupling by interacting only at the interface level.
## Composition
- A stronger form of aggregation where components are intrinsic to the class.
- Composition relationships are a subset of aggregation, highlighting inseparable connections between classes.
### Aggregation vs Composition Example
- **Aggregation**: One class contains references to another, but the contained object exists independently
- **Composition**: One class is composed of another, and the composed object cannot exist independently
## Association
- **General association**: Any interaction between classes, generally one-way.
- **Bidirectional association**: Mutual interaction where each class references the other
## Realization
- Describes the implementation of an interface or abstract class.
- **"Is-a" relationship**: The child class realizes the behavior of its parent
### Abstract Class coupling
- Often cause tighter coupling than interfaces due to shared fields, implemented methods, and constructors. A child class must interact with these implementations, creating more dependencies.
### Generalization
- Inheritance, where a parent class describes general behavior, and children specialize this behavior
- This relationship leads to the tightest coupling since changes in the parent class can affect all its children, creating resistance to change.
# Inheritance Flaws
- Inheritance seen as time-saving by allowing shared code in a parent class, aligning with the DRY principle.
- Experience changed perspective: inheritance often leads to regret as code evolves.
- encourage **aggregation over inheritance**.
## Code reuse
- Aggregation provides:
    - Clearer, more maintainable code.
    - Avoidance of unnecessary polymorphism and inheritance.
    - Reduced coupling between classes.
## The dangers of inheritance
- **Polymorphism**:
    - Powerful, but introduces tight coupling between **callers** and **implementers**.
    - Changes in abstraction impact both the callers and implementers.
- To mitigate this:
    - Keep interfaces small and stable to avoid widespread consequences from changes.
    - Avoid inheritance unless it significantly reduces complexity compared to aggregation.
## Conclusion
- **Interfaces** are preferred over inheritance when abstraction is needed, as they reduce the coupling between components.
- **Dependency injection** allows for more flexible and testable code.
- **Aggregation** is used instead of inheritance to re-use code when necessary, without forcing a hierarchical relationship between classes.
# SOLID
## Single Responsibility Principle (SRP)
- A class or module should have only one reason to change. This ensures that each part of the software is responsible for a single piece of functionality.
## Open-Closed Principle
- A software artifact should be open for extension, but closed for modifications
- avoid changing existing module code that 1) is working 2) is well tested and 3) has other modules dependent on it
- instead, extend the behavior and interface to another class that handles the new feature needs
## Liscov Substitution Principle
- All subclasses should be substitutable for their base or parent class
- every single behavior the parent class has must be inherited by the child.
## Interface Segregation Principle
- A client should never be forced to depend on an interface they do not use
- a module should only be dependent upon interfaces it needs
- to ensure this, we should not group unlike behaviors into the same interface
## Dependency Inversion
- Entities should only depend on abstractions, not on concretions.