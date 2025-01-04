---
layout: default
title: Refactoring 3
parent: Midterm Notes
nav_order: 8
---
# Functional Programming
- A paradigm where functions are treated as first-class citizens, meaning they can be assigned to variables, passed as arguments, and returned by other functions.

## Python Functional programming

```python
def my_func(input):
    print("I am a function")
    print("My input is", input)
    
x = my_func
print(x)
x(10)
```

prints: 

```python
# where in memory the function instructions are stored
<function my_func at 0x00000178068A2E18>
# calling the function
I am a function
My input is 10
```

## What Java has: SAMs
- **Functional interfaces** are interfaces that can be used to define a **single abstract method**

*example - implementations of Comparator*

```java
public class StringIgnoreCaseComparator implements Comparator<String> {
    public int compare(String s1, String s2) {
        s1 = s1.toUpperCase();
        s2 = s2.toUpperCase();
        return s1.compare(s2);
    }
}
```

## Anonymous classes
- create an instance of a new class when needed
- no name in global namespace

```java
    new Comparator<UVAStudent>() {
        @Override
        public int compare(UVAStudent o1, UVAStudent o2) {
            return Integer.compare(o1.getId(), o2.getId());
            }
    }
```

## Lambda functions
- unnamed function that is created when needed at runtime

general structure:

```java
(parameters) -> {code block} // don't need parenthesis for 1 parameter

// or

(parameters) -> expression
```

*example*

```java
Comparator<String> ignoreCase = (s1, s2) -> {
    return s1.toUpperCase().compareTo(s2.toUpperCase());
};

// or

Comparator<String> ignoreCase = (s1, s2) -> 
        s1.toUpperCase().compareTo(s2.toUpperCase());
```

## Functional interfaces to know:

### Comparator
- **Method**: `public int compare(E e1, E e2)`
- **Use**: Sorting lists (`Collections.sort` and `listInstance.sort`). Also used in defining `TreeSet`s.
- **Example**: `(a, b) -> a - b` (sorts numbers in ascending order).

### Executable (JUnit5)
- **Method**: `public void execute()`
- **Use**: Used in `assertThrows`, `dynamicTest`, `assumingThat`.
- **Example**: `() -> student.enroll(cs3140)` (Executable for enrolling a student in a class).

### Runnable (JUnit5)
- **Method**: `public void run()`
- **Use**: Describes a procedure executed by a `Thread`.
- **Example**: `Thread newThread = new Thread(() -> myRunFunction())`.

### Predicate
- **Method**: `public boolean test(E e)`
- **Use**: Check if a value meets a condition (used in `Stream.filter`).
- **Example**: `(student) -> student.getGPA() > 3.5` (returns true for students with GPA > 3.5).

### Consumer
- **Method**: `public void accept(E e)`
- **Use**: Performs an action on a value, no return. Used in `foreach` and `peek` (Java Streams).
- **Example**: `(value) -> System.out.println(value)` (prints values from a list).

### Supplier
- **Method**: `public E get()`
- **Use**: Generates results without taking input.
- **Example**: `() -> (int) (Math.random() * 6) + 1` (generates a random number between 1 and 6).

### Function<E, R>
- **Method**: `public R apply(T)`
- **Use**: Transforms an input of type `T` and returns `R`. Used in `map` (Java Streams).
- **Example**: `x -> x.toString().toUpperCase()` (converts an object to uppercase string).

### ActionListener
- **Method**: `public void actionPerformed(ActionEvent e)`
- **Use**: Handles user interactions in event-driven applications (e.g., JavaFX).
- **Example**: `e -> handleButtonPress()` (reacts to button press).

## Method Captures
- **Example**:
    - Original: `itemList.forEach(item -> System.out.println(item))`
    - Simplified with method capture: `itemList.forEach(System.out::println)`
- **Example with `Comparator`**:
    - Original code comparing `UVAStudent`s by last name and first name:

```java
    studentList.sort((x, y) -> {
            if (x.getLastName().equals(y.getLastName())) {
                return x.getFirstName().compareTo(y.getFirstName());
            } else {
                return x.getLastName().compareTo(y.getLastName());
            }
        });
````

- we can definite a function in our `StudentManager` class:

```java
    public int compareLastNameThenFirstName(UVAStudent s1, UVAStudent s2) {
        if (s1.getLastName().equals(s2.getLastName())) {
            return s1.getFirstName().compareTo(s2.getFirstName());
        } else {
            return s1.getLastName().compareTo(s2.getLastName());
        }
    }
```

- And now we can invoke that function in our lambda body:

```java
public class UVAStudentManager {
    public void sortStudentsLastNameThenFirstName(List<UVAStudent> studentList) {
        studentList.sort(UVAStudentManager::compareLastNameThenFirstName);
    }

    public static int compareLastNameThenFirstName(UVAStudent s1, UVAStudent s2) {
        if (s1.getLastName().equals(s2.getLastName())) {
            return s1.getFirstName().compareTo(s2.getFirstName());
        } else {
            return s1.getLastName().compareTo(s2.getLastName());
        }
    }
}
```

# Java Streams
- **Streams** provide a way to process collections of data in a functional programming style. 
- Think of streams as an assembly line that takes in data, processes it, and returns a result.

## Streams
**Goal:** Get the total population of all states.
Non-stream method:

```java
    public int getTotalPopulation(List<State> stateList) {
        int sum = 0;
        for (State state : stateList) {
            sum += state.getPopulation();
        }
        return sum;
    }
```

Using streams:

```java
    public int getTotalPopulation(List<State> stateList) {
        return stateList.stream()
            .mapToInt(state -> state.getPopulation)
            .sum();
    }
```

**Goal:** Get the smallest 5 states, sorted by population.
Non-stream method:

```java
    public List<State> getSmallestNStates(List<State> stateList, int numberOfStates) {
        List<State> safeCopy = new ArrayList<>(stateList);
        safeCopy.sort(new Comparator<State>() {
            @Override
            public int compare(State o1, State o2) {
                return Integer.compare(o1.getPopulation(), o2.getPopulation());
            }
        });
        List<State> smallestNStates = new ArrayList<>();
        for (int i = 0; i < numberOfStates; i++) {
            smallestNStates.add(safeCopy.get(i));
        }
        return smallestNStates;
    }
```

Using streams:

```java
    public List<State> getSmallestNStates(List<State> stateList, int numberOfStates) {
        return stateList.stream()
            .sorted(Comparator.comparing(State::getPopulation()))
            .limit(numberOfStates)
            .collect(Collectors.toList());
    }
```

## Beginning - `.stream()`
- **Start a stream** with:  `myCollection.stream()`
- Intermediate operations perform transformations on data, while terminal operations produce results like lists or sums.

## Intermediate operations

### `sorted`
- **Method**: `sorted(Comparator<E> comparator)`
- **Example**: `.sorted(Comparator.comparing(State::getName))`
- **Output**: Stream sorted based on the given comparator.
- **Explanation**: Sort elements based on a key, such as sorting states by population or name.
- **Related functions**:
- **Reversing order**: `Comparator.comparing(...).reversed()` for descending order.

### `filter`
- **Method**: `filter(Predicate p)`
- **Example**: `.filter(state -> state.getPopulation() > 1000000)`
- **Output**: Stream with only elements satisfying the predicate (e.g., filtering states with population > 1 million).

### `limit`
- **Method**: `limit(long n)`
- **Example**: `.limit(10)`
- **Output**: A stream with only the first `n` elements.
- **Usage**: Often used after sorting to get the smallest or largest `n` elements.

### `map`
- **Method**: `map(Function<E, R> f)`
- **Example**: `.map(state -> state.getName())`
- **Output**: Stream of transformed elements.
- **Usage**: Converts elements from one type to another, like extracting state names from a list of states.

### `distinct()`
- **Method**: `distinct()`
- **Example**: `.distinct()`
- **Output**: Removes duplicates from the stream based on `.equals()` method.

### `peek`
- **Method**: `peek(Consumer<E> e)`
- **Example**: `.peek(System.out::println)`
- **Output**: No changes to the stream, but executes an action (like logging).
- **Difference**: Unlike `forEach`, `peek` is intermediate.

### `flatmap`
- **Method**: `flatMap(Function<E, R> f)`
- **Example**: `.flatMap(list -> list.stream())`
- **Output**: Flattens a stream of collections into a single stream of elements.
- **Usage**: Converts nested collections into a single stream (e.g., a list of lists into a list).

## Terminal Operations
- All streams end with a single terminal operation.

### `foreach`
- **Method**: `forEach(Consumer<E> e)`
- **Output**: `void` – doesn’t return anything directly
- **Use**: Executes an action on each item left in the `Stream`.

### `count`
- **Method**: `count()`
- **Output**: `long` – number of items left in the `Stream`.

### `collect()`
- **Method**: `collect(Collector<E, A, R> c)`
- **Output**: Some `R` – an accumulation of remaining elements in the `Stream`.
- **Common Collectors**:
    - `toList()` – returns an unmodifiable `List<E>`.
    - `toSet()` – returns an unmodifiable `Set<E>`, removes duplicates.
    - `toCollection(ArrayList::new)` – returns a modifiable collection.
    - `toMap()` – creates a modifiable `Map`, throws `IllegalStateException` on duplicate keys.
    - `joining()` – joins elements of a `Stream<String>`.
    - `counting()` – equivalent to `count()`.

### average, sum
- **Methods**: `Collectors.averagingDouble`, `averagingInt`, `averagingLong`, `summingDouble`, etc.
- **Use**: Returns the average or sum of elements in a stream, with variations for different numeric types.

### max, min
- **Methods**: `max(Comparator<E> comparator)`, `min(Comparator<E> comparator)`
- **Output**: `Optional<E>` – returns an optional value.
- **Use**: Finds the maximum/minimum remaining value using a comparator.

### reduce
- **Method**: `reduce()`
- **Use**: Combines all items in a `Stream` into a single result, like summing values.

## `parallelStream`
- **Use**: Enables multi-threading for stream operations, but can process items in an unpredictable order.
- **Workaround**: Use `forEachOrdered()` for maintaining order in `parallelStream()`.
- **Performance**: Better suited for large data sources; multi-threading overhead may slow small datasets.

## Files.line
- **Use**: Creates a `Stream<String>` from file reading using `Files.lines()` or `BufferedReader.lines()`.
- **Limitations**: File reading streams are necessarily sequential and not parallelizable.