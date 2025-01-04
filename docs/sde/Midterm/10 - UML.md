---
layout: default
title: UML
parent: Midterm Notes
nav_order: 10
---
# UML Class Diagrams
- Used to communicate design plans and structure
- UML Class Diagram rules are guidelines, not strict commandments
- Best diagram is one that aids in communication
![example.png](https://sde-coursepack.github.io/modules/design/images/uml/example.png)

## Individual classes
Class diagrams show class name at the top
- Can indicate `<<abstract>>` or `<<interface>>` for abstract classes or interfaces
![classExample.png | 300](https://sde-coursepack.github.io/modules/design/UML-Class-Diagrams/..%2Fimages%2Fuml%2FclassExample.png)

### Fields
- Listed in the format `[name]:[type]`
- Common practice: aggregation, composition, or association fields not listed

### Methods
- Listed at the bottom with parameters and return types using the `[name]:[type]` format
- Constructors listed first
- Visibility indicators:
    - `+` for public items (methods and constructors)
    - `-` for private items (usually fields)
    - `#` for protected items
- Private methods often not listed since they are implementation details

### Simplified UML Diagram
- May omit fields and method names, listing only the classes

## Relationship diagrams

![relationships.png](https://sde-coursepack.github.io/modules/design/UML-Class-Diagrams/..%2Fimages%2Fuml%2Frelationships.png)

### Dependency
- Weakest form of coupling (one class uses another without containing it)
- Arrow points to the class being used (dependency)

- the arrow points from the **client** (`ClosestCenterStateFinder`) to the **dependency** (`Tweet`, `Coordinate`)
![closestCenterStateFinder.png](https://sde-coursepack.github.io/modules/design/UML-Class-Diagrams/..%2Fimages%2Fuml%2FclosestCenterStateFinder.png)

### Aggregation and Composition
- One class possesses another as a field
- Example: `ClosestCenterStateFinder` possesses multiple `State` instances
- Aggregation/composition indicated by a diamond at the owning class's side
- Multiplicity indicated (e.g., `1` for exactly one, `1..*` for one or more)

![aggregationUML.png](https://sde-coursepack.github.io/modules/design/UML-Class-Diagrams/..%2Fimages%2Fuml%2FaggregationUML.png)

### Bidirectional Association
- Mutual aggregation where both classes are aware of each other
- Lacks a diamond to show mutual ownership

![bidirectionalAssociation.png](https://sde-coursepack.github.io/modules/design/UML-Class-Diagrams/..%2Fimages%2Fuml%2FbidirectionalAssociation.png)

#### Multiplicity
- Describes the quantity relationship between classes
- Three groups:
    - **One-to-One**: Each student has one transcript
    - **One-to-Many**/**Many-to-One**: Each student has multiple assignment submissions, but each submission belongs to one student
    - **Many-to-Many**: Students enrolled in multiple courses, each course having multiple students
- Diagram notation for multiplicity:
    - `1`: exactly one
    - `1..*`: one or more
    - `0..*`: zero or more
    - `x..y`: between x and y

#### Data, not entities
- Diagrams represent data and states, not real-world objects

### Realization/Generalization
- Represents inheritance (classes implementing interfaces or extending other classes)
- Triangular arrow points to the class being extended or implemented
- Dotted line for realization (interfaces), solid line for generalization (class inheritance)

![interfaces.png](https://sde-coursepack.github.io/modules/design/UML-Class-Diagrams/..%2Fimages%2Fuml%2Finterfaces.png)

## Criticisms of UML Diagrams
- Some developers find UML diagrams time-consuming and not always useful
- Benefits for newer developers:
    - Helps form mental connections in software design
    - Similar to how learning LISP improves function-writing skills
- Diagrams help communicate new software systems but are secondary to effective code documentation
- Overuse of UML diagrams is not recommended
- Can be used for quick sketching to solidify design ideas but should not be strictly translated into code
- Not a vital industry skill; however, basic understanding is useful for communication purposes