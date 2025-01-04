---
layout: default
title: Refactoring 2
parent: Midterm Notes
nav_order: 7
---
Extract Method: [https://sde-coursepack.github.io/modules/refactoring/Extract-Method/](https://sde-coursepack.github.io/modules/refactoring/Extract-Method/)**

# Analyzability
- **Analyzability** is a vital aspect of software evolution. As our software grows and changes, we need to understand code we have already written so we can maintain it, update it, and re-use it.
- **Readability** is the part of analyzability that relates to being able to correctly interpret the **syntax** of software.
- **Understandability** is the part of analyzability that relates to understand the semantics, or the **high-level meaning** of the code, and why the code exists.
- Using good code style and human-readable identifier names improves _readability_
- Using _effective_ identifier names, avoiding “magic values” that require additional context to explain, and good class design can improve understandability.

# “Good Code”
- Varies across languages (Java vs. Python/C++) due to style and syntax differences.
- Goes beyond functionality; emphasizes understandability, maintainability, and scalability.
**Software Entropy**
- Older software is harder to maintain (fix bugs, add features, update infrastructure).
- Changes in older code may cause unrelated features to break (example of bug fix causing issues).
- Empirically, older software takes more time to implement changes, but this increase can be slowed.
**Technical Debt**
- "Cleaning up" applies to code as it does to physical tools (e.g., cleaning up after installing a fan).
- Leaving messy, complex code adds technical debt, which costs more time later (e.g., using global variables unnecessarily).
- Technical debt, like financial debt, accrues interest: delaying fixes makes the problem worse.
**Managing Technical Debt**
- Early, one-time investments (e.g., refactoring) drastically reduce future maintenance time.
- Postponing clean-up leads to worse consequences and longer fix times.
**Starting Over from Scratch**
- Avoid the temptation to rewrite code from scratch due to technical debt.
- Refactoring should be ongoing to prevent "Big Redesign in the Sky" scenarios.
- The best time to improve code is right after writing it; the worst is "later."
**You Always Read More Than You Write**
- Writing code involves reading existing code much more than actually writing new code.
- Clean, readable code improves the overall efficiency of a project.
- Spending time on refactoring reduces complexity and slows down project entropy.
**"But this will slow me down..."**
- Clean, well-written code increases long-term speed; messy code slows future development.
- Bob Martin’s analogy: slowing down early (e.g., refactoring) leads to faster progress later, similar to racing cars around corners.
- Avoiding shortcuts early prevents greater slowdowns in the future.
**Taking Pride in Your Work**
- Programming is a powerful tool; aspire to be the best developer you can be.
- Clean, understandable code reflects professionalism and care for your craft.
- Feeling pride in your work and code quality benefits both you and others who may use your code.

# Code Smells
**Long Methods**
- Overly long methods are difficult to read, maintain, and reuse.
- Functions should do one thing only; long methods often do too much.
- Benefits of breaking up methods:
	1. Easier to test, reuse, and modify.
	2. Clear answers to questions like "How do we read the file?" in specific methods.
**Large Classes**
- Common in new programmers who place everything in one class.
- Problems arise when:
    1. Too many unrelated fields or methods.
    2. Classes grow too long, making them hard to understand.
- Decompose large classes into smaller, focused ones with specific responsibilities.
**Long Parameter Lists**
- Clean Code suggests keeping arguments to a minimum (preferably zero, one, or two).
- Functions with more than three arguments (polyadic) should be avoided.
- Refactor with classes, overloaded constructors, or patterns like Factory/Builder if needed.
**Boolean Parameters**
- Avoid using boolean parameters as they increase the cognitive load for the function's caller.
- Solution: Split into two distinct functions
- Boolean arguments lead to unclear intent, requiring more knowledge to use functions correctly.
**Polymorphism as a Solution**
- Consider using polymorphism/separate classes instead of booleans.
- This approach follows the **Strategy Pattern**, separating data from its processing logic.
**Primitive Obsession**
- A class with too many fields using primitive data types complicates code.
- Solution: Group related fields into smaller, more focused classes 
- Benefits: Reduced complexity, more modular, easier to maintain and understand.
**Duplicate Code**
- Avoid repeating similar logic in multiple places. It increases the risk of bugs.
- Solution: Use helper methods and polymorphism to eliminate duplicated code, simplifying updates and reducing error risks.
**Message Chains**
- Long, complex chains of method calls are hard to read and maintain.
- Solution: Break the chain into smaller, focused methods. This improves readability and makes code easier to test and maintain.

# Refactoring
- Refactoring improves the **internal** structure of code without changing its external behavior. It enhances design, analyzability, testability, and maintainability.

## Testing and Refactoring
- Regression tests are critical to ensure that external behavior remains unchanged.
- Tests act as scaffolding for refactoring, ensuring that internal improvements don’t break functionality.
- **Extract constants**: Replace magic numbers with named constants
- **Renaming Identifiers**: use Refactor→Rename in IntelliJ
- **Renaming Classes**: Class names should reflect their purpose
- **Moving classes between packages**: In IntelliJ, drag and drop a class to move it; import statements and test classes will be updated
- **Change Signature**: In IntelliJ, you can refactor method signatures without causing syntax errors.
- **Array/Objects**: Refactor arrays into objects for better maintainability.\
- **Inline variable**: Remove unnecessary if-statements when they contain a boolean expression.
- **Abstract conditional logic**: Move complex boolean expressions into helper functions.
- **Invert boolean**: Refactor negative boolean logic to positive for improved readability.
- **Error Codes**: Avoid using error codes; instead, throw exceptions for better error handling.
- **Replace Exception with Optionals**: Use `Optional<T>` to represent the potential absence of a value, rather than throwing exceptions.

```java
    public int max(List<Integer> integerList) {
        if (integerList.isEmpty()) {
            throw new IllegalArgumentException("Cannot get max of empty list!");
        }
        int maximum = integerList.get(0);
        for (Integer value : integerList) {
            if (value > maximum) {
                maximum = value;
            }
        }
        return maximum;
    }
```

vs.

```java
    public Optional<Integer> max(List<Integer> integerList) {
        if (integerList.isEmpty()) {
            return Optional.empty();
        }
        int maximum = integerList.get(0);
        for (Integer value : integerList) {
            if (value > maximum) {
                maximum = value;
            }
        }
        return Optional.of(new Integer(maximum));
    }
```

- **Replace Null with Optionals**

```java
    public Student getStudentByComputingId(String computingId) {
        for (Student student : studentList) {
            if (student.getComputingID.equals(computingId)) {
                return student;
            }
        }
        return null;
    }
```

vs.

```java
    public Optional<Student> getStudentByComputingId(String computingId) {
        for (Student student : studentList) {
            if (student.getComputingID.equals(computingId)) {
                return Optional.of(student);  
            }
        }
        return Optional.empty();
    }
```

use of function:

```
    Optional<Student> optionalStudent = getStudentByComputingId("abc2def");
    if (optionalStudent.isPresent()) {
       Student student = optionalStudent.get();
       //do something with student
    } else {
       //no student with that computingId exists
    }
```

- **Preserve Whole Object**: Pass objects (like `Coordinate`) instead of individual fields to avoid primitive obsession and improve code readability.
- **Getters for mutables return copies**: When returning mutable objects, return a copy using a constructor of `clone()` to prevent accidental modification.
- **Making copies of Collections**: Return a copy of a mutable collection (e.g., `ArrayList`) to avoid unintended modifications.

### Extracting Methods
- **Purpose of Extracting Methods**:
    - Break large methods into smaller ones for better clarity and functionality.
    - Improve code readability by encapsulating logic into methods with meaningful names.
    - Eliminate duplicate code by centralizing repeated logic in a single method.
- **Breaking Up Big Methods**:
    - Functions should ideally perform one task. A practical example: calculating the distance between points from a file.
- **Extracting Methods in IntelliJ**:
    - Use the "Extract Method" tool to break down long sections of code into smaller, more manageable methods.
- **Well-Written Prose**:
    - Code now reads like a clear sequence of steps.
    - Each method handles a distinct responsibility, improving maintainability.
- **Final Structure**:
    - Main method reads more clearly with higher-level abstractions.
    - Low-level details (e.g., argument parsing, display formatting) are extracted into their own methods (`getFilenameFromArguments()`, `displayTotalDistance()`).
- **Benefits of Extracting Methods**:
    - **Readability**: Clear separation of concerns.
    - **Isolation of Changes**: Changes in logic only affect specific methods without disturbing the entire flow.
    - **Testability**: Each method can be tested independently.