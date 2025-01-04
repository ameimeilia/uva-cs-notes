---
layout: default
title: Testing
parent: Midterm Notes
nav_order: 5
---
# Verification and Validation
- **Verification**: Ensures that code meets the specification. Focuses on whether the code performs as intended, with all features implemented and tests passing.
- **Validation**: Ensures that the product satisfies customer needs. Focuses on whether the software solves the customer's real-world problem.
- **Key Differences**:
    - Verification is internal (developer-focused) and checks if the product was built correctly.
    - Validation is external (customer-focused) and checks if the right product was built.
- **Defects, Failures, Mistakes, Errors**:
    - **Defect**: An undiscovered problem in the product.
    - **Failure**: Inability of the software to function as specified.
    - **Mistake**: A human error, like a bug or misunderstood customer need.
    - **Error**: The difference between the current and correct state.
- **Importance of Finding Defects Early**:
    - Early detection prevents wasting time and effort on incorrect designs and implementations.
    - The cost of fixing defects increases exponentially the longer they exist.
    - Testing frequently and early (e.g., test-driven development) minimizes time spent searching for and fixing defects.

# Testing
- **Primary Goal**: Find defects.
- **Mindset**: Assume your code has bugs, even in simple cases.
- **Testing Limitation**: Ideal cases might not reveal all defects; edge cases can still be faulty.
- **Testing Scenario**: Controlled setup with inputs and expected outputs.
    - **Pass**: Expected and actual results match.
    - **Fail**: They don’t match.
- **Starting Point**: Testing in the `main` method is simple but not scalable for large projects.
- **Why Test**: Detect hidden bugs before they cause major issues.

# Types of Testing

### Unit Testing
- **Definition:** Tests individual modules or functions.
- **Purpose:** Quickly identifies small, simple bugs.
- **Focus:** Test non-trivial public methods; automate when possible.
- **Advantages:** Isolates bugs, aids debugging, and is easy to automate.

### Integration Testing
- **Definition:** Tests how combined modules work together.
- **Automation:** Often automated with tools like JUnit 5; complex cases may need special tools.

### System Testing
- **Definition:** Tests the entire system from the user's perspective.
- **Challenges:** Difficult to pinpoint failures; should not be the only testing method.

### Regression Testing
- **Definition:** Ensures fixes do not reintroduce old bugs.
- **Automation:** Tools like Gradle can automate this process.

### Continuous Integration (CI)
- **Definition:** Tests code when merged into protected branches.
- **Tools:** GitHub Actions, Travis CI, AWS; GitHub Actions offers ~2000 free minutes/month.

### Testing Based on Audience
- **Internal Testing:** Done by developers to ensure functional and technical correctness.
- **Alpha Testing:** Internal system testing to simulate real user behavior.
- **Beta Testing:** External user testing to gather feedback and find issues.
- **Acceptance Testing:** Validates the software meets customer needs and satisfaction.

# JUnit 5
- **JUnit 5 & Gradle**
    - JUnit 5 incorporated via Gradle dependencies:
        - `testImplementation 'org.junit.jupiter:junit-jupiter-api:5.8.1'`
        - `testRuntimeOnly 'org.junit.jupiter:junit-jupiter-engine:5.8.1'`
    - Gradle `test` task uses JUnit Platform for testing execution.
- **Why not use `main` method for testing?**
    - Manual effort to check test results by reading output.
    - `main` method might have other execution responsibilities.
    - Inability to automate or run multiple tests easily.
    - A dedicated testing framework like JUnit is more scalable and automated.
- **Test Class Structure**
    - Testing classes are organized parallel to the classes being tested.
        - Source code in `src/main/java`.
        - Test code in `src/test/java`.
    - Gradle uses test classes during testing, but excludes them during execution, keeping testing and execution isolated.
- **Test Methods in JUnit**
    - Test methods are used to verify code functionality.
    - A test **fails** if it crashes or throws an `AssertionError`.
    - A test **passes** if the code executes successfully without an error.
- **Test Method Example**
    - Example:

```java
@Test
@DisplayName("Testing 3-argument max with Descending order arguments: 3, 2, 1")
public void testMaxDescendingArguments() {
    int a = 3, b = 2, c = 1;
    int expected = 3;
    int actual = MaxFunctions.max(a, b, c);
    assertEquals(expected, actual);
}
```

- **@Test Annotation**
    - Marks a method as a test for the JUnit framework.
    - **Conditions for `@Test` methods:**
        1. Must be `public` and not `static`.
        2. Must return `void`.
        3. Cannot take in arguments.

### Key Notes on Assertions in JUnit
- **Assertions**: Used to verify that a test's results match expectations. If an assertion is incorrect, an `AssertionError` is thrown, causing the test to fail.
#### `assertEquals`
- **Purpose**: Checks if two values are equal. If they aren't, the test fails.
- **Usage**:
    - Input: The expected value comes first, followed by the actual value.
    - Test passes if the expected and actual values match.
    - Example:

```java
assertEquals(3, MaxFunctions.max(3, 2, 1));
```

#### `assertEquals` with Objects
- **Objects**: When comparing objects, `assertEquals` uses the `.equals()` method. Arrays, for example, compare references, not content.
- Use `assertArrayEquals` to compare array contents.

#### Floating Point Numbers
- **Floating-point imprecision**: Testing doubles requires a tolerance factor due to imprecision in floating-point arithmetic.
    - Use `assertEquals(expected, actual, tolerance)` for comparison.
    - Example: `assertEquals(2.0, 2.0000000004, 1e-9)` passes with tolerance.

#### `assertTrue` and `assertFalse`
- **Purpose**: Use these to verify boolean expressions.
    - `assertTrue(condition)` ensures the condition is `true`.
    - `assertFalse(condition)` ensures the condition is `false`.
    - Downsides: Default error messages can be uninformative. Add custom messages for clarity.

#### `assertNull` and `assertNotNull`
- **Purpose**: Check if a value is `null` or not.
    - `assertNull(x)` ensures `x` is `null`.
    - `assertNotNull(x)` ensures `x` is not `null`.

#### `assertThrows`
- **Purpose**: Used to verify that a specific exception is thrown by a method.

### Testing Documentation
- **Java Tools** can help make test output more readable and actionable.
- **@DisplayName** allows tests to have more descriptive names, aiding in debugging.
    - Display names should help quickly identify:
        - What function/module is being tested.
        - What input is used.
        - What a test failure indicates.
    - Example: `@DisplayName("Testing 3-argument max with Descending order arguments: 3, 2, 1")`
- **Test Fail Messages**:
    - `assert` functions in Java allow optional fail messages.
    - Custom fail messages make it easier to identify which assertion failed.
    - Helps with debugging especially in tests with multiple assertions.
    - Example with fail message: `assertEquals(4, myList.size(), "Incorrect size of list");`

### Benefits of Testing
- **@DisplayName** helps quickly identify failing tests and assists in debugging.
- **Improved Code Construction Efficiency**:
    - Testing as you write code is more efficient than waiting until code is finished.
    - Waiting to test until after coding increases debugging time.
    - Initial testing may slow you down, but practice improves speed and efficiency.
- **Modular Code**:
    - Writing tests encourages more modular functions.
    - Shorter, more focused functions are easier to test and debug.
- **Tests as Documentation**:
    - Tests provide usage examples and documentation for how to use classes and methods.
    - Example: Tests show how to initialize `MySortedList` and add values to it.

### Why We Need Automated Testing
- **Onboarding**: Automated testing aids in onboarding new developers by providing a safety net.
- **Refactoring**: Automated tests help ensure that refactoring does not introduce new bugs.
- **Evolution**: Automated testing helps ensure that evolving software continues to provide existing functionality.
- **Bug Fixes**: Automated tests are crucial for verifying that bug fixes resolve issues without disrupting other parts of the software.
- **Continuous Development**: Automated testing is the backbone of iterative software development.
- **Skill Development**: Effective testing is a key skill that supports reliable and maintainable software development.

# Testing with Objects

### Different Testing Methods
- **Testing State Changes**:
    - **Example**: `NumberChanges` class tracks a number and how many times it has been changed.
    - **State Before Operation**: Check the initial state (e.g., `nc.number = 7`, `nc.changes = 4`).
    - **State After Operation**: Perform an operation (e.g., `nc.setNumber(13)`), then verify changes (e.g., `nc.number = 13`, `nc.changes = 5`).
    - **Testing Recipe**:
        1. **Create and Configure**: Initialize the object to a starting state.
        2. **Execute**: Perform the operation to be tested.
        3. **Check State**: Verify the state of the object to ensure it changed as expected.
- **Example Test Case**:

```java
@Test
void testAlreadyChangedSetNumberChanged() {
    NumberChanges nc = new NumberChanges(7, 4);
    nc.setNumber(13);
    assertEquals(13, nc.getNumber(), "Number did not change to 13 correctly");
    assertEquals(5, nc.getTimesChanged(), "Number of times change did not correctly increment");
}
```

- **Second Test Case**:
    - **Scenario**: Testing behavior when the number does not change (e.g., `nc.setNumber(7)`).
    - **Expected Behavior**: Number remains the same, changes count remains unchanged.

```java
@Test
void testAlreadyChangedSameNumber() {
	NumberChanges nc = new NumberChanges(7, 4);
	nc.setNumber(7);
	assertEquals(7, nc.getNumber(), "Number changed when it should have stayed 7");
	assertEquals(4, nc.getTimesChanged(), "Number of times changed when it shouldn't have");
}
```

### Good Testing Rules
- **One Operation Per Test**: Focus on testing one operation at a time to isolate issues and avoid complicating tests.
- **Test the Interface, Not the Implementation**: Test public methods and their behaviors rather than internal details, which maintains test relevance despite implementation changes.
- **Ensure Sound Tests**: Ensure tests accurately reflect specifications, avoid false positives and negatives, and verify failing tests before debugging.

### One Assertion Per Test
- **Concept**: Prefer one assertion per test to easily identify failures. However, this can lead to redundant code and reduced readability.

### Only Call the Tested Operation Once
- **Concept**: Avoid multiple calls to the method under test in a single test to clearly identify the cause of any failures.

### Don’t Test Private Methods
- **Concept**: Focus on public and protected methods only, as private methods are implementation details and may change.

### Setting Up Test Objects
- **Concept**: Avoid redundant setup code by using setup methods to initialize test objects.

```java
class MySortedListTest {
    MySortedList myAddTestList;

    @BeforeEach
    public void setupMyAddTestList() {
        ArrayList<Integer> starterList = new ArrayList<>();
        starterList.add(1);
        starterList.add(3);
        starterList.add(5);
        myAddTestList = new MySortedList(starterList);
    }

    @Test
    public void addValueToMiddle() {
        myAddTestList.add(4);
        // Assertions...
    }
}
```

### What Methods to Test
- **Concept**: Test all public and protected methods, focusing on the class interface rather than implementation details.

### How Many Tests to Write
- **Concept**: Write tests based on the level of certainty needed, covering various cases and edge conditions.

### Accessing Private Fields
- **Concept**: Avoid direct access to private fields; use alternative methods like getters or protected fields sparingly, as they may compromise encapsulation.

### Testing with Reflections
- **Concept**: Java Reflection can access private fields/methods but increases complexity and coupling to implementation details.

### Mocking
- **Concept**: Use mocks for external dependencies to isolate the class under test, though it requires a deeper understanding and is covered separately.