---
layout: default
title: Midterm Study Guide
parent: Midterm Notes
nav_order: 11
---
# Software Engineering

## Ad hoc development
- ad hoc development is a one time solution that is fundamentally different from building systematic solutions

## Software complexity
- accidental complexity emerges due to circumstance
- essential complexity is intrinsic to developing software
- the “hard part” of writing software is the specification, design, and testing of the conceptual construct

## Software Quality
- external quality: from the stakeholder perspective
	- functionality, reliability, usability, efficiency, portability
- internal quality: from the developer perspective
	- analyzability, changeability, stability, reusability, testability

# Java

## Compiling vs. Interpreting
- high level source code is compiled into an byte-code executable for the computer to run
- interpreting: compiling and running is done at the same time
- interpreted languages are less efficient than compiled languages

## .java, .class, and .jar files
- `.java`: Java code file
- `.class`: produced after compiling
- `.jar`: contains several files that can be run as a single Java class

## Basic java command line arguments
- `java`: runs a program
- `java -jar`: runs a jar file
- `javac`: compiles java file

## How Java works
- JDK: compiles `.java` files and creates `.class` files, which are portable between environments
- JRE: runs `.class` files
- JVM: contained in the JRE, handles direct interactions with hardware, interprets `.class` byte code
- JIT: compiles JVM byte-code into native machine code

## Benefits of this approach
- compiling with JDK allows sharing of executables across different OS that can be run with JRE/JVM
- JIT allows for optimization
- overall convenience, distributability, portability, and performance

## Packages
- Java code is organized into packages of related or interacting classes
- packages must be imported, ex. `import java.util.*`
- to create a package: `package [name of package]` at top of file

## Command Line Arguments
- command line arguments are stored in `String[] args`
- `List<String> argsList = Arrays.toList(args);`
- optional arguments are preceded with 1 or 2 hyphens, i.e. `-arg`, `--arg`

# Software construction

## git version control
- **Distributed repository**: every workstation has a local copy of the repository
![[Screenshot 2024-09-05 at 8.53.46 PM.png | center | 350]]
- **`git` commands**:
	- `add`: adds staged changes to a file to the local repository
	- `commit`: save changes to the local repository
	- `push`: pushes to the remote repository
	- `pull`: pulls updates from the remote repository
	- `branch`: create a new branch to work in
	- `switch`/`checkout`: switch to a branch
	- `merge`: merge 2 branches
- **Conflict resolution**: conflict occurs when conflicting changes have been pushed since your last pull
- Using **branches** effectively
	1. One branch per feature
	2. Merge effectively and safely: merge main into feature, resolve conflicts, then merge feature into main

## Gradle
- **Build Tool**: simplifies the process of using libraries and building `.jar` files
- **Dependency management**: add external libraries to the dependencies section of the `build.gradle` file
- **`build`**: downloads dependencies, compiles code, runs tests, builds output jar file
- **`test`**: defines how to run tests
- **`fat-jar`**: contains code and all dependencies
- **`gradlew`**: installs correct Gradle for that project

# Testing

## Primary purpose of testing
- to find defects

## Test Scenarios
- **Input**
	- Parameters: check that inputs to functions are valid
	- State: check the conditions of the object before executing the function
- **Output**
	- Expected vs. Actual: ensure the actual result matches the expected result
	- Return value: check for correct type and value
	- State: check how the conditions of the object change after executing the function
- **Design Test Scenarios for both Functions and Objects Methods**
	- **Functions**: test for input parameters, return values, and independent state
	- **Object Methods**: test for input/output as well as the object state before/after the function

## Writing JUnit tests
- `@Test`: marks a method as a test case
- `@BeforeEach`: sets up common objects/states before running tests
- **Assertions**
	- `assertEquals`: verifies that expected and actual values are equal
		- primitives
		- double: use with a tolerance for floating-point
		- Objects: use `.equals` method
	- `assertTrue` / `assertFalse`: checks if a condition is true or false
	- `assertThrows`: checks whether a specific exception was thrown

```java
    @Test
    public void withdrawInsufficientFundsException() {
        assertThrows(RuntimeException.class, () -> testAccount.withdraw(600));
        assertEquals(500, testAccount.getBalance(),1e-4);
    }
```

- Types of **Test Scenarios**
	1. **Equivalence**: divide input with similar behavior into equivalence classes then test representative values from each partition
	2. **Boundary**: test for edge cases
	3. **Exception**: test that correct exceptions are thrown
	4. **Robustness**: test beyond expected input range
- Test plans
	- **Black-box Testing**: test external functionality
		- Exhaustive: test all possible inputs, not feasible
		- Random testing: test randomly selected inputs, less reliable
		- Equivalence Partitioning: divide input into groups with similar behavior and test each group
	- **White-box Testing**: test internal functionality
		- Code coverage: check that specific parts of code are executed
			- Statement: ensure every statement is executed
			- Branch: ensure all possible branches are executed
			- Conditional: ensure all boolean expressions are tested
			- Path: ensure every possible execution path is tested
		- Running with Coverage: Tools like IntelliJ IDEA or Eclipse offer coverage reports that show how much of the code has been executed during testing.
- **Test-Driven Development**: write tests before writing code
	1. write a failing test
	2. write just enough code so that the test passes
	3. refactor the code and repeat

# Defensive Programming

## Throwing Exceptions
- use exceptions to enforce **pre** and **post-conditions**
- avoid using exceptions for **control-flow**

## Handling Exceptions
- only handle exceptions when you can **meaningfully respond**
- Java syntactically forces you to handle checked exceptions
- handle checked exceptions in one place
- throw checked exceptions as unchecked exceptions

## Making your own Exceptions
- usually use `RuntimeException` class

```java
public class InsufficientFundsException extends RuntimeException {
    public InsufficientFundsException(String message) {
        super(message);
    }
}
```

## Protecting Class Usage
- **Defensive programming** ensures your code handles unexpected input and states gracefully, making it more robust and error-resistant.
- Prevent **invalid states** by checking inputs (e.g., throwing an exception if invalid data is passed).

## AssertionError
- **`assert`**: used in tests to check conditions, `assert [boolean_statement]
- `-ea`: add to **VM arguments** to enable assertions

# Code Quality

## Analyzability
- ability to understand code in order to maintain, update, and reuse it
- **Readability**: ability to interpret the syntax of the software
- **Understandability**: ability to understand the high-level meaning of the code
- Readability is a necessary but insufficient condition for understandability

## Code Smells
1. **Long functions**: difficult to read, maintain, and reuse; functions should only do one thing
2. **Long parameter list**: avoid functions with 3+ arguments
3. **Boolean parameters**: lead to unclear intent; write 2 distinct functions instead
4. **Primitive Obsession**: class with too many primitive data fields; write more focused classes
5. **Message Chains**: chains of method calls are hard to read and maintain

## Refactoring techniques
1.  **Renaming Identifiers**: update variables and function names to reflect their purpose
	- code shouldn’t require pausing to understand simple ideas
2. **Introduce constant**: named constants improve understandability
3. **Abstract conditional logic**: move complex boolean expressions into helper functions
4. **Invert boolean**: make all boolean logic positive
5. **Replace ErrorCode with Exception**: use exceptions for better error handling
6. **Replace Null with Optional**: avoid nulls by using `return Optional.empty`
7. **Extract method**: ensure methods only have one purpose
8. **Extract class**: break up functionality into classes that handle different, specific behavior
9. **Preserve Whole Object**: passing objects avoids primitive obsession

## DRY vs. WET
- **DRY**: don’t repeat yourself
- **WET**: write everything twice

## Code Comments
- **Comments**: explains why a specific line of code was written a particular way, useful for resolving bugs; short-term
- **Documentation**: explains high-level flow of the code, overview, and guidelines; long-term

## Technical Debt
- delaying fixing code will make the problem harder to fix in the future
- refactor early and often to reduce future maintenance times

# Functional Programming

## Lambda functions in Java
- unnamed function that is created when needed at runtime, usually passed as arguments to functions
- implements functional interfaces

general structure:

```java
(parameters) -> {code block} // don't need parenthesis for 1 parameter

// or

(parameters) -> expression // no need for return keyword
```

*example*

```java
Comparator<String> ignoreCase = (s1, s2) -> {
    return s1.toUpperCase().compareTo(s2.toUpperCase());
};

// or

Comparator<String> ignoreCase = (s1, s2) -> 
        s1.toUpperCase().compareTo(s2.toUpperCase());

List<String> words = new ArrayList<>(List.of("Apple", "Zebra", "banana", "Catfish", "aardvark"));
words.sort((s1, s2) -> s1.toUpperCase().compareTo(s2.toUpperCase()));
System.out.println(words);
```

## Functional Interface
1. **Comparator**: used to sort lists, used for `.sorted()` and `.sort()`
2. **Consumer**: take in a value, do something with it, but don’t return, used for `forEach()`

```java
// replace
for(Item item : itemList) {
    System.out.println(item);
}

// with
itemList.forEach(item -> System.out.println(item))
```

3. **Predicate**: checks if a value meets a condition, used for `filter()`

```java
(student) -> student.getGPA() > 3.5 // returns true for students with GPA > 3.5
```

4. **Function**: transforms an input of type `T` and returns `R`, used for `map()`

```java
x -> x.toString().toUpperCase() // takes object x and returns String

d -> (int)(d + 0.5) // takes Double and returns an Integer
```

5. **Executable**: defines an executable, used for` assertThrows()` to check for exception

```java
() -> student.enroll(cs3140)
```

## Java Streams
- **Streams** are used to process collections of data in a functional programming style, focusing on **declarative** processing rather than **imperative** loops

**Making Streams from** 
1. List

```java
List<State> stateList = new ArrayList<>();
Stream<State> stateStream = stateList.stream();
```

2. Set

```java
Set<State> stateSet = new HashSet<>();
Stream<State> stateStream = stateSet.stream();
```

3. Map (via entrySet())

```java
Map<String, State> stateMap = new HashMap<>();
Stream<Map.Entry<String, State>> entryStream = stateMap.entrySet().stream();
```

**stream() vs parallelStream()**
- **stream()**:
    - Runs sequentially on a single thread.
- **parallelStream()**:
    - Runs operations in parallel using **multiple threads**.
    - Use parallelism carefully; it is most beneficial for large datasets.
    - **Cost of multithreading**: Parallel streams have overhead from thread management and synchronization, which can make them slower on smaller data sets.

**Intermediate operations**: Operations that transform a stream and return another stream for further processing
- **`sorted`**: Sorts elements of the stream using a comparator

```java
.sorted(Comparator.comparing(State::getPopulation))
```

- **`map`**: Transforms each element in the stream.

```java
.map(state -> state.getName())
```

- **`filter`**: Filters elements based on a condition.

```java
.filter(state -> state.getPopulation() > 1000000)
```

- **`limit`**: Limits the number of elements in the stream.

```java
.limit(5)
```

- **`peek`**: Performs an action on each element without modifying the stream.

```
.peek(System.out::println)
```

- **`distinct`**: Removes duplicates.

```
.distinct()
```

**Terminal Operations**: Operations that end the stream and produce a result.
- **`forEach`**: Performs an action for each element in the stream.

```java
.forEach(state -> System.out.println(state.getName()))
```

- **`count`**: Returns the number of elements in the stream.

```java
long count = stateList.stream().count();
```

- **`toList`**: Turn elements of stream into list

```java
List<State> smallestStates = stateList.stream()
    .sorted(Comparator.comparing(State::getPopulation))
    .limit(5)
    .collect(Collectors.toList());
```

- **`reduce`**: Combines all elements into a single result.

```java
int totalPopulation = stateList.stream()
    .map(State::getPopulation)
    .reduce(0, Integer::sum);
```

# Software Design:
- good software design slows software entropy and reduces the overall effort required to make a software change

## Design Concepts
- **Modularity**: breaking big problems into smaller, manageable parts
	- **Single responsibility principle**: one purpose per module
- **Functional Independence**: modules should have minimal interaction with other parts of the system
- **Abstraction**: hide unnecessary details, keep implementation details hidden behind the interface
	- changes to implementation should not affect other parts of the system if the interface remains unchanged
- **Information Hiding**: enforce abstraction by limiting what is exposed to users of a module, prevents clients from directly accessing internal data

## Cohesion vs. coupling
- **Highly Cohesive**: module is focused on a specific purpose, parts are interdependent and logically related; high intra-dependency
- **Loosely Coupled**: interaction between multiple modules is low; low-interdependency
- **Cohesion** from worst to best:
	1. **Coincidental Cohesion**: elements grouped randomly
	2. **Logical Cohesion**: elements share broad activity
	3. **Temporal Cohesion**: elements grouped based on timing
	4. **Procedural Cohesion**: elements groups based on order of execution
	5. **Communicational Cohesion**: elements grouped based on shared data
	6. **Sequential Cohesion**: output of one step is input to the next
	7. **Functional Cohesion**: modules focus on a single, well-defined task
- **Coupling** from worst to best:
	1. **Content Coupling**: One class modifies the private state of another class directly
	2. **Common Coupling**: modules have **read and write** access to the same global data
	3. **Control Coupling**: one module passes control information (like booleans or enums)
	4. **Stamp Coupling**: module passes a large data structure, but only part of it is used.
	5. **Data Coupling**: Pass only the necessary data, and return only the required result.

## Class relationships 
- **Dependency**: occurs when one class uses methods or functions from another
	- arrow points to class being used
![[Screenshot 2024-10-08 at 2.47.17 PM.png | center | 100]]
- **Aggregation/Composition**
	1. **Aggregation**: occurs when a class "has" instances of other classes as fields.
	2. **Composition**: a stronger form of aggregation where components cannot exist meaningfully outside their parent
	- diamond is closer to owner side
![[Screenshot 2024-10-08 at 2.47.45 PM.png | center | 100]]
![[Screenshot 2024-10-08 at 2.50.06 PM.png | center | 100]]
- **Bidirectional Association**: mutual interaction where each class references the other, usually also having mutual aggregation
![[Screenshot 2024-10-08 at 2.48.09 PM.png | center | 150]]
- **Realization**: when a class implements an interface
	- arrow points to class being implemented
![[Screenshot 2024-10-08 at 2.48.50 PM.png | center | 200]]
- **Generalization**: inheritance, when a class inherits properties and behaviors from another class
	- arrow points to class being extended
![[Screenshot 2024-10-08 at 2.49.13 PM.png | center | 100]]
- **"Prefer Composition/Aggregation over Inheritance"**: loose coupling, re-use code when necessary, fewer hierarchies
- **Inheritance Flaws**: increased coupling, harder to make changes as code evolves
- **Single Responsibility Principle**: A class or module should have only one reason to change. This ensures that each part of the software is responsible for a single piece of functionality.

# Polymorphism
**Benefits of Polymorphism**:
- **Extensibility**: You can introduce new subclasses without altering existing code that uses parent types.
- **Code Reusability**: Generic code can work with different subclasses and interfaces.
- **Loose Coupling**: It promotes the use of abstractions, reducing dependencies on specific implementations.

**Dependency Inversion**
- Entities should only depend on abstractions, not on concretions
- flexible, maintainable design where the high-level code isn’t affected by changes in low-level code.