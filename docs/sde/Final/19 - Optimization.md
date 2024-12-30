---
layout: default
title: Optimization
parent: Final Notes
nav_order: 10
---
Code Aesthetics: [https://www.youtube.com/watch?v=tKbV6BpH-C8](https://www.youtube.com/watch?v=tKbV6BpH-C8) 
Step-by-step optimization video: [https://www.youtube.com/watch?v=U16RnpV48KQ](https://www.youtube.com/watch?v=U16RnpV48KQ) 
This video shows how to solve this problem: [https://adventofcode.com/2022/day/6](https://adventofcode.com/2022/day/6)**
# Optimization
## Trade-offs
- **Principles from Economics**:
    - **Trade-offs**: Choices involve giving up one thing for another (e.g., features vs. cost).
    - **Opportunity cost**: The value of the next best alternative you give up. Example: Choosing between buying a GPU or saving the money for long-term investments.
- **Paradox of Choice**:
    - More options can lead to regret, even if the same choice would be made with fewer options.
## Analysis Paralysis
- **Definition**: Being so focused on finding the "best" solution that no progress is made.
- **Approach**:
    - Prefer releasing an imperfect, improvable product efficiently rather than waiting for perfection.
    - Software should respond to change; perfection today may become irrelevant tomorrow.
## Irrelevant Optimization
**Intro Programming Example**:
- Students often ask whether a `for` loop or `while` loop is faster. Answer: Focus on writing **working code first**.
- Emphasis on **bug prevention** over micro-optimization.
**Efficiency Perspective**:
- Small performance differences (e.g., between `for` and `while` loops) are less significant than avoiding bugs.
- Prioritize debugging and maintainability unless performance becomes critical.
## Code Trade-offs
- **Efficiency vs. Other Goals**:
    - Beyond efficiency, software must also be:
        - Functionally correct, robust, usable, portable.
        - Internally maintainable, analyzable, and testable.
    - Example: Abstractions (e.g., `HashMap` to `TreeMap`) improve flexibility at the cost of performance.
- **Memory Management Trade-offs**:
    - **Java**: Uses garbage collection for simplicity but sacrifices efficiency.
    - **C, C++, Rust**: Require manual memory management for better performance but are harder to program in.
- **Optimization Context**:
    - Efficiency matters but should not come at the cost of simplicity or other qualities unless necessary.
    - Common issues stem from:
        - Using inefficient algorithms (e.g., selection sort vs. merge sort).
        - Using incorrect data structures (e.g., `List` vs. `Set` or `Map`).
    - Optimize only when performance problems arise, not as a default approach.
## Don’t listen to me
### Critical vs. Noncritical
- Noncritical code: Sections that don’t significantly affect overall performance.
- Critical code: Sections consuming the most time overall, requiring focused optimization.
### Critical section
- **Objective**: A program searches for a word/phrase in "Adventures of Sherlock Holmes" and prints line numbers with matches.
- **Benchmarking Results**:
    - Searching "Internet Memes" (non-existent): ~25-30ms.
    - Searching "qzj" (shorter, non-existent): ~27-35ms.
    - Searching "The" (frequent): ~40-45ms.
#### Code Inefficiency Analysis
- **Original Issue**:
    - `target.toLowerCase()` called repeatedly in the loop unnecessarily.
    - Adjustments:
        - Convert `target` to lowercase once after input:
	        - `String target = scanner.nextLine().toLowerCase();`
		- Compare directly in the loop:
			- `if (line.toLowerCase().contains(target)) { ... }`
- **Results**:
    - Minimal improvement for frequent term "The" (~39-41ms).
    - No improvement for "Internet Memes" (~25-30ms).
#### Why “The” Takes Longer
- Likely due to:
    - **Increased Matches**: Searching "The" involves processing 5317 matches.
    - **String Operations**: Adding results to the `results` string repeatedly inside the loop.
### String Concatenation
- **Issue**: Concatenating `String` in a loop (`results += currentLineNumber + ", ";`) is inefficient due to Java's handling of `String`.
	- Concatenation creates a new memory allocation of combined length (`n + n`) and copies characters one by one.
	- Runtime for concatenating two `String`s is **O(n)**.
	- In a loop, as the `String` grows longer, the operation's complexity increases, resulting in an overall **O(k² * n)** time complexity for `k` concatenations.
### StringBuffer
- **Why use `StringBuffer`?**
	- Functions like an `ArrayList`, preallocating space and doubling its size when necessary.
	- Amortized time for each character addition is **O(1)**, making the total runtime **O(k * n)** for `k` concatenations.
	- Significantly improves performance in scenarios with repeated concatenations.

```Java
StringBuffer resultsBuffer = new StringBuffer();
for(String line = bufferedReader.readLine(); line != null; line = bufferedReader.readLine()) {
	if (line.toLowerCase().contains(target)) {
		resultsBuffer.append(currentLineNumber)
				.append(", ");
		count++;
	}
	currentLineNumber++;
}
```

- `append`: Adds argument’s `String` representation to the buffer.
- `toString`: Returns buffer content as a `String`.
- **Performance**:
    - Improvement from 39–41ms to 27–32ms (~25% faster).
    - Loop efficiency enhanced significantly since the loop runs thousands of times.
### Noncritical improvements
- **Misconception**: Replacing all `String` concatenations with `StringBuffer` will drastically improve performance.
- **Reality**:
    - Replacing non-critical concatenations (like in `message` and `endingMessage`) yields negligible improvements.
    - Reasons:
        1. **Low Count**: Only 8 concatenations occur in these sections, too few to make an impact.
        2. **Overhead**: `StringBuffer` introduces memory allocation and function call overhead (`append` and `toString`).
        3. **Java Optimizations**: Java already optimizes sequential `String` concatenations, minimizing performance gains for `StringBuffer` in non-critical sections.
    - **Key Insight**: Focus optimization efforts where code is executed frequently.
### Finding the Critical Section
- **General Rule**: The more a piece of code is executed, the more critical it is to optimize.
    - Inner loops or frequently called functions are key targets.
- **Profiling Tools**:
    - Example: IntelliJ Ultimate Profiler.
    - Highlights:
        - Which lines of code take the longest to execute.
        - Helps identify “hot spots” (e.g., string concatenation inside the loop).
## The Lesson
- **Lesson**: Avoid over-optimizing non-critical sections; it rarely provides meaningful improvements.
- **Best Practices**:
    - Identify and optimize critical sections where the majority of time is spent.
    - Use appropriate data structures and algorithms for more substantial improvements.
    - Avoid “micro-optimizations” unless profiling highlights them as bottlenecks.
# Optimization Example
## Point Class
## [Point.java](https://github.com/sde-coursepack/EfficiencyExample/blob/master/src/main/java/edu/virginia/cs/sde/efficiency/Point.java)

```Java
public class Point {
    private double x, y;

    public Point(double x, double y) {
        this.x = x;
        this.y = y;
    }
    ... //getters and setters
    //Euclidean distance
    public double distanceTo(Point point) {
        double xDiff = this.getX() - point.getX();
        double yDiff = this.getY() - point.getY();
        return Math.sqrt(xDiff * xDiff + yDiff * yDiff);
    }
}
```

## [Path.java](https://github.com/sde-coursepack/EfficiencyExample/blob/master/src/main/java/edu/virginia/cs/sde/efficiency/Path.java)

```Java
public interface Path {
	// note there is one add method for doubles and one for a Point instance
    void add(double xCoordinate, double yCoordinate);
    void add(Point point);
    Point get(int index);
    int size();
    double totalDistance();
}
```

## [PointListPath.java](https://github.com/sde-coursepack/EfficiencyExample/blob/master/src/main/java/edu/virginia/cs/sde/efficiency/PointListPath.java)

```Java
public class PointListPath implements Path {
    private final List<Point> points;

    public PointListPath(ArrayList<Point> points) {
        this.points = points;
    }
    // default array of size 10
    // means that `ArrayList.add` still has an amortized constant runtime
    public PointListPath() {
        points = new ArrayList<>();
    }

    public PointListPath(int initialCapacity) {
        points = new ArrayList<>(initialCapacity);
    }

    @Override
    public void add(double xCoordinate, double yCoordinate) {
        points.add(new Point(xCoordinate, yCoordinate));
    }

    @Override
    public void add(Point point) {
        points.add(point);
    }

    @Override
    public Point get(int index) {
        return points.get(index);
    }

    @Override
    public int size() {
        return points.size();
    }

    @Override
    public double totalDistance() {
        double totalDistance = 0.0;
        //Iterate through all but the last point, getting distance to next point
        for (int i = 0; i < size()-1; i++) {
            Point firstPoint = points.get(i);
            Point secondPoint = points.get(i + 1);
            totalDistance += firstPoint.distanceTo(secondPoint);
        }
        return totalDistance;
    }
}
```

## Explaining the solution
![pathListBetterDiagram.png](https://sde-coursepack.github.io/modules/refactoring/Optimization-Example/..%2Fimages%2Foptimization%2FpathListBetterDiagram.png)
- `Point` objects **are not actually inside the `PointListPath`** object at all
- a `PointListPath` variable is a **reference** to a `PointListPath` object in memory
- tells where to **look** for its contents, not the contents themselves
- ex. getting the `x` coordinate of the point at index `2` involves the following:
	1) Go to the memory address referenced and access the reference value of `points`  
	2) Go to the memory address `points` references and access the reference value of `contents` plus 8 bytes (at least, typically 8 bytes, since each reference is 4 bytes “wide”)  
	3) Go to _that_ memory address and access the `double` value of `x`
### Before you panic and burn down your code
- There is nothing _wrong_ about this code
- but be aware of is that jumping around memory locations _takes time_
- remember **_premature optimization is the root of all evil_**
### Where this hurts performance
- Arrays store all contents sequentially in order
- Every time we call a constructor using `new`, the JRE allocates a location in memory for our data
- creating objects with `new` doesn’t mean objects are adjacent in memory, even if references are stored in sequential memory
- always have to take at least one extra step in memory to access a value.
## [CoordinateArrayPath.java](https://github.com/sde-coursepack/EfficiencyExample/blob/master/src/main/java/edu/virginia/cs/sde/efficiency/CoordinateArrayPath.java)
- avoid the need for references → store the values of x and y in the same array

```Java
public class CoordinateArrayPath implements Path {
    private final double[] pointArray;
    private final int length;
    private int currentSize;

    public CoordinateArrayPath(int capacity) {
        pointArray = new double[capacity * 2];
        this.length = capacity;
        this.currentSize = 0;
    }

    @Override
    public void add(double xCoordinate, double yCoordinate) {
        if (currentSize == length) {
            throw new IllegalStateException("CoordinateArray is full and cannot add more points");
        }
        pointArray[currentSize * 2] = xCoordinate;
        pointArray[currentSize * 2 + 1] = yCoordinate;
        currentSize++;
    }

    @Override
    public void add(Point point) {
        add(point.getX(), point.getY());
    }

    @Override
    public Point get(int index) {
        if (index >= currentSize) {
            throw new IndexOutOfBoundsException(String.format(
                    "index - %d is out of bounds for CoordinateArray of size - %d", index, currentSize));
        }
        double pointX = pointArray[2 * index];
        double pointY = pointArray[2 * index + 1];
        return new Point(pointX, pointY);
    }

    @Override
    public int size() {
        return currentSize;
    }

    @Override
    public double totalDistance() {
        double totalDistance = 0.0;
        for (int i = 0; i < currentSize - 1; i++) {
            double diffX = pointArray[2 * i] - pointArray[2 * i + 2];
            double diffY = pointArray[2 * i + 1] - pointArray[2 * i + 3];
            totalDistance += Math.sqrt(diffX * diffX + diffY * diffY);
        }
        return totalDistance;
    }
}
```

- store the `x` value of `2 * i`, and `y` at `2 * i + 1`
- utilizes spatial locality of the processor cache

```Java
// the array of
[3, 1, 4, 7, -5, 2]
// represents the points
(3, 1), (4, 7), (-5, 2)
```

## Benchmarking
- measure timing of two approaches
- benchmarks:
	1) **Building** the two `Path` implementations (using `add`) from a list of two `double` arrays (one for x coordinates, one for y coordinates).  
	2) **Calculating the total distance of the path**
### Distance Benchmark

|Input Size|PointListPath|CoordinateArrayPath|
|---|---|---|
|10|0ms|0ms|
|100|0ms|0ms|
|1000|0ms|0ms|
|10000|1ms|1ms|
|100000|2ms|2ms|
|1000000|3ms|2ms|
|10000000|19ms|15ms|
|100000000|199ms|153ms|
- `CoordinateArrayPath` _is_ about 25% faster than `PointListPath`.
- **but** the actual raw time difference is only 46ms
### Add Benchmark

|Input Size|PointListPath|CoordinateArrayPath|
|---|---|---|
|10|0ms|0ms|
|100|0ms|0ms|
|1000|0ms|0ms|
|10000|1ms|1ms|
|100000|2ms|1ms|
|1000000|14ms|1ms|
|10000000|144ms|18ms|
|100000000|1294ms|166ms|
- `PointPathList` is much slower than `CoordinateArrayPath` due to memory allocation.
- `CoordinateArrayPath` is slightly over 87% faster.
### Total Runtimes

|Input Size|PointListPath|CoordinateArrayPath|SpeedUp|
|---|---|---|---|
|10|0ms|0ms|N/A|
|100|0ms|0ms|N/A|
|1000|0ms|0ms|N/A|
|10000|1ms|1ms|0%*|
|100000|4ms|3ms|75%*|
|1000000|16ms|4ms|75%*|
|10000000|163ms|33ms|80%|
|100000000|1493ms|319ms|78%|
### Do we care NOW?!?!
- speed up is negligible especially when accounting for simplicity, understandability, and modularity
- prioritize code understandability and flexibility over performance until performance becomes a problem
## Some optimization heuristics
- **Heuristics**: Rules-of-thumb for optimization; always measure changes to verify improvement.
- **Key Practices**:
	1. **Compare Approaches**: Test multiple implementations objectively.
	2. **Isolate Benchmarks**: Separate the tested code from unrelated parts.
	3. **Use Large Inputs**: Test with significant input sizes (e.g., tens of thousands+).
	4. **Match Real Usage**: Optimize for typical input sizes your system handles.
	5. **Run Multiple Tests**: Avoid relying on single runs; results can be skewed.
	6. **Vary Test Order**: Test in different sequences (A then B, B then A) to avoid bias.
	7. **Consider Environment**: Account for heat, ambient temperature, and hardware setup.
# ArrayList vs LinkedList
- change constructor to:

```Java
    public PointListPath(int initialCapacity) {
        points = new LinkedList<>();
    }
```

|Input Size|ArrayList|LinkedList|
|---|---|---|
|10|0ms|0ms|
|100|0ms|0ms|
|1000|0ms|0ms|
|10000|1ms|59ms|
|100000|2ms|6061ms|
|1000000|3ms|N/A|
|10000000|19ms|N/A|
|100000000|199ms|N/A|

- In general, if you have a LinkedList that you need to iterate through, you should always use an Iterator, so that getting each next node in the list is a constant time operation
- adjusting code to account for Iterators makes it more complicated:

```Java
    public double totalDistance() {
        double totalDistance = 0.0;
        Iterator<Point> iterator = points.listIterator();
        Point currentPoint = iterator.next();
        while (iterator.hasNext()) {
            Point nextPoint = iterator.next();
            double distance = currentPoint.distanceTo(nextPoint);
            totalDistance += distance;
            currentPoint = nextPoint;
        }
        return totalDistance;
    }
```

|Input Size|ArrayList|LinkedList w/ get(index)|LinkedList w/ iterator|
|---|---|---|---|
|10|0ms|0ms|1ms*|
|100|0ms|0ms|0ms|
|1000|0ms|0ms|0ms|
|10000|1ms|59ms|0ms|
|100000|2ms|6061ms|4ms|
|1000000|3ms|N/A|202ms|
|10000000|19ms|N/A|1717ms|
|100000000|199ms|N/A|N/A|
- **references are slower than data**
- each node of the LinkedList is a `Node` object which has separate references to the `Point` and the next `Node`
- Java cannot leverage the cache with the LinkedList because the data is not sequential
- `LinkedList` also comes up with a significant memory overhead due to storing `Point`s and `Node`s, which contain two references
## What are LinkedLists even for?
- the only time LinkedLists are even theoretically better than ArrayLists is adding/removing from the front of the structure
- adding at the front of a LinkedList is constant time , or O(1), while for an ArrayList it’s linear time, or O(n)

|Size|LL add|AL add|LL rem|AL rem|LL tot|AL tot|
|---|---|---|---|---|---|---|
|10|0ms|0ms|0ms|0ms|0ms|0ms|
|100|0ms|0ms|0ms|0ms|0ms|0ms|
|1000|0ms|1ms|0ms|0ms|0ms|1ms|
|10000|0ms|4ms|1ms|4ms|1ms|8ms|
|100000|2ms|316ms|1ms|281ms|3ms|597ms|
|1000000|132ms|97609ms|10ms|55569ms|142ms|153178ms|
- notice that `add`ing on a `LinkedList` is slower than `removing` due to needing to allocate memory allocation
## Built-in optimizations
- when sorting a `LinkedList`, Java Collections puts the contents of the `List` into an array, and then sorts the array
- After the array is sorted, it simply sets the value of each element into the List to the value that matches its index in the sorted array
- sorting both an `ArrayList` and a `LinkedList` use O(n * log(n)) time

|Size|LL sort|AL sort|
|---|---|---|
|10|0ms|0ms|
|100|0ms|0ms|
|1000|0ms|0ms|
|10000|4ms|2ms|
|100000|31ms|19ms|
|1000000|372ms|261ms|

# Optimization Patterns
## My approach to optimization
- **Double-check data structures**: Ensure you're using the right ones.
- **Profile and focus on outliers**: Focus on functions that use the most processor time, not small-impact ones.
- **Check algorithms**: Consider more optimized algorithms or libraries.
- **Work in a new git branch**: This allows safe experimentation and comparison.
## DRY - Don’t Recalculate Yourself
- consider the following code:

```Java
public Path getShortestPath(List<Path> paths){
    Path shortestPath = paths.get(0);
    for (Path path:paths) {
        if (path.totalDistance() < shortestPath.totalDistance()) {
            shortestPath = path;
        }
    }
    return path;
}
```

- always **re-calculating** the distance of `shortestpath` over every iteration of the loop
- **trade** `calculation-time` for `memory` by storing the `shortestDistance` as a variable

```Java
public Path getShortestPath(List<Path> paths){
    Path shortestPath=paths.get(0);
    double shortestDistance = shortestPath.totalDistance();
    for (Path path:paths) {
        var pathDistance = path.totalDistance();
        if (pathDistance < shortestDistance) {
            shortestPath = path;
            shortestDistance = pathDistance;
        }
    }
    return path;
}
```

### Nested Arrays
- consider the following code:

```Java
public class Matrix {
	...
    
    public void add(Matrix other) {
        for (int row = 0; row < height; row++) {
            for (int column = 0; column < width; column++) {
                this.matrix[row][column] += other[row][column];
            }
        }
    }
}
```

- `other[row][column]` follows 3 different references:
	1) From the function scope to the `other` object’s memory location  
	2) From the `other` location to it’s `matrix` array of `int[]` 
	3) From the `other.matrix[row]` reference to the underlying array
- using variables to limit references:

```Java
    public void add(Matrix other) {
        for (int row = 0; row < height; row++) {
            thisRow = this.matrix[row];
            otherRow = other.matrix[row];
            for (int column = 0; column < width; column++) {
                thisRow[column] += otherRow[column];
            }
        }
    }
```

## Lazy Evaluation
- **Eager Evaluation Example:**
    - The code calculates the total distance of a path every time a new coordinate is added, resulting in unnecessary recalculations (e.g., calculating the distance 5 times when only the last distance is needed).
    - **Problem**: Redundant calculations waste time, especially when only the final result is used.
- **Lazy Evaluation:**
    - Avoids evaluating a function or expression until its result is needed.
    - Prevents unnecessary recalculations and optimizes performance, especially for functions or computations that are called repeatedly but whose intermediate results aren’t used.
    - **Trade-off**: Lazy evaluation can be beneficial in many cases, but it might increase complexity when there are dependencies or when values change unexpectedly.
### Lazy Initialization
- **Lazy Initialization**: Delays the creation of an object until it’s actually needed.
    - **Example**:
        - Initializing `thing` in the constructor only when `getThing()` is called, rather than initializing it immediately.
    - **Benefits**:
        - **Memory Efficiency**: Avoids allocating memory for objects until needed.
        - **Cost Efficiency**: Particularly useful if the object is large or expensive to create.
    - **Drawbacks**:
        - The first access to the object will be slower.
        - More complex code due to two possible states for the object (initialized or not).
    - **Use Case**: Ideal for objects that might not be needed in all scenarios, like loading large datasets or objects conditionally.
## Memoization
- **Memoization**: Storing the result of an expensive computation so that future calls with the same arguments can return the cached result, improving performance.
- **Example (Path Object)**:
    - The `CoordinateArrayPath` class computes total distance once and stores it in `distanceMemo`
    - On subsequent calls to `totalDistance()`, the cached value is returned instead of recalculating the distance.

```Java
if (distanceMemo.isPresent()) {
    return distanceMemo.get();
}
```

- **Trade-off**:
	- **Memory Usage**: Storing cached values takes up memory.
	- **Time Efficiency**: Significant improvement for functions that are repeatedly called with the same parameters.
### Memo invalidation
- Memoized results can become outdated if underlying data changes. For instance, calling `add()` on `Path` alters the total distance, necessitating invalidation of the cached value.
- **Invalidate Memo**: Reset the `distanceMemo` whenever the object’s state changes that affects the memoized value.

```Java
distanceMemo = Optional.empty(); // Invalidate previous memo on change
```

### Memoization with arguments
**Memoization with Arguments**:
- **Example**: In the `FileSummer` class, memoization is extended to handle functions with arguments by using a `Map` to store results.

```Java
private Map<String, Long> fileSumMemo;
public long getFileSum(String filename) {
    if (fileSumMemo.containsKey(filename)) {
        return fileSumMemo.get(filename);
    }
    long fileSum = 0L;
    // Calculation logic here
    fileSumMemo.put(filename, fileSum);
    return fileSum;
}
```

**Trade-off**:
- If the underlying data changes (e.g., file content changes externally), the memoized value might become incorrect. This leads to potential issues with stale or inaccurate results.
### Memo Invalidation Challenges
- **Memoization Invalidation**: A critical challenge in using memoization is ensuring that cached values are invalidated when the underlying data changes. Failing to do so can lead to bugs and performance issues.
- **Quote on Cache Invalidation**:
    - "There are only two hard things in Computer Science: cache invalidation and naming things." — Phil Karlton.
    - While cache invalidation can be difficult, it’s essential to ensure the correctness and efficiency of memoization-based solutions.
## Conditional Optimizations
#### Early Checking of Pre-Conditions
- The original function checks both `path` and `pathMap` for null before performing expensive operations like calculating `totalDistance()`. However, the calculation of `totalDistance` is wasteful if `pathMap` is null, as it's not used.
- **Optimization**: Check the "easy" conditions (null checks) first to prevent unnecessary operations.  
    **Improved Code**:

```Java
public void addToPathMap(Path path, Map<Path, Double> pathMap) {
    if (path == null) {
        throw new RuntimeException("Error: Null Path");
    }
    if (pathMap == null) {
        throw new RuntimeException("Error: Null Path");
    }
    var totalDistance = path.totalDistance();
    pathMap.put(path, totalDistance);
}
```

- This way, we avoid calculating `totalDistance()` if `pathMap` is null.
### Conditional Order
- In a function that checks for a reservation's validity:

```Java
public boolean isReservationValid(Reservation reservation, Time currentTime) {
    return reservationService.reservationExists(reservation) &&
        reservation.isTimeInWindow(currentTime);
}
```

- **Optimization**: Since `reservation.isTimeInWindow(currentTime)` is a fast operation (no remote database query), it should be checked **first**. This takes advantage of **short-circuiting** in boolean expressions:

```Java
public boolean isReservationValid(Reservation reservation, Time currentTime) {
    return reservation.isTimeInWindow(currentTime) &&
        reservationService.reservationExists(reservation);
}
```

- This way, if the first condition is false, the second condition is never evaluated, saving time and resources.
#### Short-Circuiting and Operator Differences
- **`&&` (short-circuiting)**: Stops evaluating as soon as one condition is false.
- **`&` (non-short-circuiting)**: Evaluates both conditions, regardless of the first condition.
- Always prefer `&&` unless both conditions must be evaluated for side effects.
### Likelihood Conditional Order
- If both conditions in a boolean expression are expensive but one is more likely to be true or false, reorder them based on likelihood to maximize short-circuiting efficiency.
	- Example: If `d()` is more likely to be false than `e()`, check `e()` first.

```Java
if LikelyFalse() || LikelyTrue() // Better performance if LikelyFalse() is more likely.
```

- Similarly, if the first condition is likely true, and both conditions are costly, check the more likely condition first to save computation:

```Java
if LikelyTrue() && LikelyFalse() // Better performance if LikelyTrue() is more likely.
```
