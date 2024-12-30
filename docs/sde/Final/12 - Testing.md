---
layout: default
title: Testing
parent: Final Notes
nav_order: 2
---
# Software Integration
- Interfaces and abstract classes allow for subverting the traditional “top-down” implementation from the `main` method.
- Interfaces and inheritance enable a “plug-and-play” architecture, reducing dependencies between high-level and low-level classes.
- Individual module functionality is insufficient; modules must work correctly when combined.
- **Integration**: Combining different software modules and ensuring they work together.
## Integration Strategies
- Integration involves combining modules to ensure correct collective behavior.
- More relationships exist between classes than the classes themselves, meaning more integrations for testing.
- Choosing the right integration strategy is crucial to avoid **integration hell**.
### Integration Hell
- **Definition**: When integrating modules takes more time and effort than developing them from scratch.
- **Symptoms**:
    - Changes in one class (e.g., Class B) force changes in dependent classes (e.g., Class A).
    - Cascading dependency issues arise from poor integration planning.
- **Avoidance**:
    - Use interfaces and abstract classes for **coding by contract** to enforce standardized integration.
    - Ensure both the syntax and semantics of interfaces are correct
### Big-Bang Integration
- **Definition**: Integration of modules all at once or in a haphazard order, often without a plan.
- **When to Use**: Suitable for small, stable programs that don’t change often.
- **Disadvantages**:
    - Not scalable for large projects.
    - Difficult to isolate the source of defects as many modules are integrated simultaneously.
## Integrating Around Dependencies
### Bottom-up
- **Definition**: Start by integrating low-level modules first and move upwards to higher-level modules.
- Integrate starting with the class **that depends on nothing**. Integrate each class after all of its dependencies have been integrated.
#### Advantages
- Can integrate and test modules as they are developed.
- Fault isolation is easier, as issues are likely related to the most recently integrated module.
- Tests can be designed to go deep into the functionality of the integrated modules.
#### Disadvantages
- High-level modules (which are often critical) are tested last.
- May result in changing high-level classes to accommodate lower-level ones, violating good design principles.
- Contradicts the natural flow of starting with high-level abstractions and breaking them into more concrete features.
- Makes prototyping difficult, as it’s easier to demonstrate a functional interface with fake data than an interface-less prototype.
### Top-down
- **Definition**: Integration starts with the class on which nothing depends and continues with classes that are depended upon by integrated classes.
- Use **stubs** to simulate unimplemented dependencies for testing purposes.
#### Top-Down mocking
- **Purpose**: Use mocks to simulate dependencies, allowing tests to be run even when the dependent classes are not yet implemented.
- The mocked data allows for testing basic functionality without fully integrating or implementing the dependency.
#### Advantages of Top-Down
- **High-level design focus**: Integrate and test high-level classes first, allowing early decisions about the design.
- **Flexibility**: Mocks provide flexibility, allowing testing of modules even with unintegrated or unimplemented dependencies.
- **Quick process**: Once familiar with mocking, the process is efficient and allows for testing without building real class objects.
#### Disadvantages of Top-Down
- **Testing user interfaces**: Difficult to mock user input, making UI testing challenging.
- **Lower-level module testing**: Low-level modules are often integrated and tested last, resulting in fewer integration tests for these components.
- **Mocking external resources**: Hard to mock interactions with external resources such as files, databases, and web services.
- **Overkill for small apps**: Mocking might be unnecessary for small applications with few modules to integrate.
### Sandwich Integration
- **Definition**: Combines bottom-up and top-down integration strategies.
- Start with a target layer then either move upwards or downwards
#### Advantages of Sandwich Integration
- **Scalability**: Useful for larger applications, as it supports integrating new modules and subprojects.
- **Isolating the User Interface**: Allows isolating the UI, which is typically the most volatile part, while still testing other modules.
#### Disadvantages of Sandwich Integration
- **Interdependencies**: Struggles with large numbers of interdependencies between modules, as identifying a clear "target layer" for integration becomes difficult.
# Mocking Objects
- **Purpose**: Mock objects allow us to test specific functions in isolation, without relying on their dependencies.
- This helps test higher-level code independently from lower-level dependencies.
## Testing more precisely
- **Goal of Unit Testing**: Test a single unit of code, ideally without external dependencies.
- **Common Mistake**: When integrating a module into the system during development, we may unintentionally include dependencies, leading to tests that aren't true unit tests.
## Avoiding False Positives
- **False Positives**: Tests can fail due to external factors, leading to misleading results.
    - Too many false positives can cause developers to ignore test failures.
- **Ideal Unit Test**: Fails only if there’s a defect in the specific code being tested.
## Integration Tests vs. Unit Tests
- **Integration Tests**: Valuable, but not substitutes for unit tests. They test how components work together, including external dependencies.
- **Unit Tests**: Should be independent and free of external dependencies.
## Test Doubles
- Replace external dependencies with objects that simulate their behavior.
- Allows for unit testing without relying on actual external systems, such as APIs.
## Stubs
- **Stub Object**: A special object that replaces an existing object of the same interface, typically returning hardcoded values.
- save the stub class in the test folder, not the source folder
- **Benefit**: Testing the target method without relying on external dependencies or APIs ensures the test only focuses on the method's logic.

*example*

```Java
public class StubNBATeamReader extends NBATeamReader {
    public static final NBATeam LAKERS = new NBATeam(1,"Lakers","Los Angelos","LAL",
            Conference.WESTERN, Division.PACIFIC);
    public static final NBATeam CELTICS = new NBATeam(2, "Celtics", "Boston", "BOS",
            Conference.EASTERN, Division.ATLANTIC);

    public List<NBATeam> getNBATeams() {
        return generateFakeNBATeamList();
    }

    private List<NBATeam> generateFakeNBATeamList() {
        return List.of(LAKERS, CELTICS);
    }
}
```

### Manual Stub Limitations
1. **Limited Flexibility**:
    - The stub is designed to test one specific method. If we need to test other methods (e.g., handling teams with state names like Utah Jazz), we would need to modify the existing stub, which might affect other tests.
2. **Creating Multiple Stub Classes**:
    - A new stub class could be created for each test, but this leads to clutter, as the number of stub classes increases.
### Need for a More Flexible Solution
- **Stub Shortcomings**: Manually creating and updating stub classes is inefficient for testing different scenarios.
- **Solution**: Use a more dynamic approach for stubs, such as Mockito, which allows creating stubs without defining entire classes.
# [Mockito](https://site.mockito.org/)
- Mockito simplifies testing by allowing “on-the-fly” stubbing, avoiding the need to create multiple stub classes.
- Reduces the amount of extra code needed and prevents pollution of the global namespace.
## Mockito usage example

```Java
package edu.virginia.cs.nbateams;

import org.junit.jupiter.api.*;
import java.util.*;

import static org.junit.jupiter.api.Assertions.*;
import static org.mockito.Mockito.*;

public class GoodAbbreviationsTest {
    private NBATeamReader reader;

    private static NBATeam LAKERS, CELTICS;

    @BeforeAll
    public static void init() {
        LAKERS = new NBATeam(1,"Lakers","Los Angelos","LAL",
                Conference.WESTERN, Division.PACIFIC);
        CELTICS = new NBATeam(2, "Celtics", "Boston", "BOS",
                Conference.EASTERN, Division.ATLANTIC);
    }

    @BeforeEach
    public void setup() {
        reader = mock(NBATeamReader.class);
    }

    @Test
    public void testGoodAbbreviations() {
        when(reader.getNBATeams()).thenReturn(List.of(LAKERS, CELTICS));

        GoodAbbreviations abbreviations = new GoodAbbreviations();

        List<NBATeam> expected = List.of(CELTICS);

        List<NBATeam> goodAbbreviationsTeams = abbreviations.extractGoodAbbreviationTeams(reader);
        assertEquals(expected, goodAbbreviationsTeams);
    }
}
```

## Stub Test Double
- `mock(NBATeamReader.class)` creates a Test Double object, acting as the `NBATeamReader` during testing.
## Writing a stub with mockito
- **Example**: `when(reader.getNBATeams()).thenReturn(List.of(LAKERS, CELTICS));`
- This stubs the `getNBATeams()` method to return a list with specific NBA teams when called.
## Installing mockito
- Add to Gradle dependencies:

```Java
    testImplementation 'org.mockito:mockito-core:4.11.0'
    testImplementation 'org.mockito:mockito-junit-jupiter:4.11.0'
```

**Version 4 over 5**: For Java 17, use Mockito 4.x (e.g., 4.11.0), as Mockito 5 requires additional setup.
### Import statements
`import static org.mockito.Mockito.*;`
## Mocks
- A mock is similar to a stub, created to remove external dependencies while monitoring interactions with those dependencies.
### Mockito `verify` function
- Used to verify that a particular function, with a particular argument, was called on a mock object

```Java
verify(mockObject).methodName(arguments);
```

### Purpose of `verify`
- **Post-Condition Checks**: Similar to `assert` methods, but specifically for mocked objects where real behavior does not exist.
### `verify` vs. `assert` methods
### Best Practices
- **Use of `verify`**: Focus on verifying side-effect producing functions only.
- **Avoid Verifying Non-Side-Effect Methods**: Do not use `verify` on methods like `contains`, `size`, or `isEmpty` that do not produce side effects.
## Why use test Doubles
- **Debugging Ease**: Debugging smaller pieces of code is easier than larger integrations.
- **Stable Tests**: Tests become stable and portable; they don’t rely on other classes, the internet, or external servers.
- **Interface Stability**: Tests remain valid as long as the interface remains unchanged, even if class implementations change.
- **Mockito Advantages**: Allows testing without creating extra dependencies, typically easier than managing real interactions.