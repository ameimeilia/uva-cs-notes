---
layout: default
title: Design Patterns
parent: Final Notes
nav_order: 6
---
# Design Pattern Background

## What's a design pattern?
- general solutions to recurring problems in software design
- conceptual solutions you adapt to fit specific issues in your program.
- **Patterns vs. Algorithms**:
    - Algorithms: Step-by-step instructions to achieve a defined goal
    - Patterns: High-level solutions, similar to blueprints that outline what’s needed

### What does the pattern consist of?
- **Intent**: Briefly describes the problem and its solution.
- **Motivation**: Elaborates on the problem and the solution provided by the pattern.
- **Structure**: Outlines classes involved and their relationships.
- **Code Example**: Demonstrates the pattern in a common programming language.

## History of patterns
- **Origins**: Concept first outlined by Christopher Alexander in "A Pattern Language" for urban design, later adapted for programming by the "Gang of Four" (Erich Gamma, John Vlissides, Ralph Johnson, and Richard Helm).
- **GoF Book**: "Design Patterns: Elements of Reusable Object-Oriented Software" (1994), introduced 23 patterns for object-oriented design, popularizing the approach.

## Why should I learn patterns?
- **Problem-Solving Toolkit**: Patterns provide tried and tested methods to address common software design issues.
- **Efficient Communication**: Patterns create a shared language for discussing solutions with teammates, making ideas easier to communicate (e.g., "use a Singleton").

## Criticism of patterns
- **Workarounds for Language Limitations**: Patterns often address limitations in a language’s abstraction level
- **Inefficient Solutions**: Strictly following patterns without adapting them can lead to inefficient solutions.
- **Overuse**: Novices may apply patterns inappropriately, complicating situations where simpler code would suffice ("if all you have is a hammer, everything looks like a nail").

## Classification of patterns
- **Complexity & Scope**: Patterns vary in detail and their effect on system design:
    - **Idioms**: Low-level patterns specific to a programming language.
    - **Architectural Patterns**: High-level, universal patterns that influence an application’s entire structure.

### Main Categories by Intent
1. **Creational Patterns**: Offer ways to create objects, enhancing flexibility and code reuse.
2. **Structural Patterns**: Describe methods to assemble objects/classes into larger structures while ensuring efficiency.
3. **Behavioral Patterns**: Focus on effective communication and responsibility distribution among objects.

# Design Patterns To Know
- Singleton: one class only has one instance and is accessible globally
- Simple Factory: returns different types of objects based on input
- Builder: used to create objects with numerous properties
- Factory: create objects in superclass but subclasses can alter the type
- Abstract Factory: create families of related objects without defining concrete classes
- Iterator: traversing collections without exposing underlying structure
- Strategy: defining a family of algorithms that are interchangable at runtime
- Observer: subscription mechanism that notifies subscribers about changes to a state
- Adaptor: converts one interface to a compatible one by wrapping
- Facade: simplified interface to a library, framework, or complex set of classes
- Decorator: attach new behaviors to objects by wrapping them

## Creational Patterns

### Singleton

####  Intent
- ensure that a class has only one instance, while providing a global access point to this instance.
- **Problem Solving**: Addresses two issues (though this violates the _Single Responsibility Principle_):
    1. **Single Instance Control**: Useful when needing controlled access to a shared resource (e.g., a database).
        - If a second object creation is attempted, it returns the same instance.
        - Regular constructors can’t achieve this, as they always create new instances by design.
        - _Note_: Clients may not realize they are always interacting with the same instance.
    2. **Global Access Point**: Offers the benefits of a global variable while safeguarding the instance.
        - Provides access from anywhere in the program but protects the instance from being overwritten.
        - Centralizes the control of instance management, avoiding scattered implementation.
- **Modern Use**: Many refer to anything addressing either problem as a "singleton."

####  Solution
- **Private Constructor**: Prevents the use of `new` to create a second instance.
- **Static Creation Method**: Acts as the constructor, calling the private constructor once and caching the object.
    - All calls to this static method return the same cached instance.

####  Structure

![The structure of the Singleton pattern](https://refactoring.guru/images/patterns/diagrams/singleton/structure-en-indexed.png?id=b0217ae066cd3b757677d119551f9a8f)

- The **Singleton** class declares the static method `getInstance` that returns the same instance of its own class.
- The Singleton’s constructor should be hidden from the client code. Calling the `getInstance` method should be the only way of getting the Singleton object.

####  Applicability
- **Use Cases**:
    - When a class should have only one instance across the application, e.g., a shared database object.
    - To control access and creation of global variables, ensuring only one instance exists.
- **Control Over Creation**:
    - Singleton blocks all other creation methods, using a single special creation method (`getInstance`).
    - This method creates the instance only once; subsequent calls return the same object.
    - Singleton can be adjusted to allow multiple instances if needed by modifying `getInstance`.

####  How to Implement
- **Private Static Field**: Add this to store the Singleton instance.
- **Public Static Creation Method**: Provides access to the Singleton instance.
- **Lazy Initialization**: Ensure the instance is created only on the first call to the static method.
- **Private Constructor**: Restricts object creation; only the static method can invoke the constructor.
- **Client Code Updates**: Replace direct calls to the constructor with calls to the static creation method.

#### Pros
- Guarantees a single instance of the class.
- Provides a global access point to that instance.
- Singleton is created only when it’s first accessed.

#### Cons
- **Violates Single Responsibility Principle**: Addresses both instance control and access.
- **Potential Design Pitfalls**: Can obscure poor design, as components might depend too heavily on each other.
- **Thread-Safety**: Requires additional handling in multithreaded environments to prevent multiple instances.
- **Testing Challenges**: Difficult to mock or unit test due to private constructor and static methods.

### Simple Factory

#### What is a Simple Factory?
- The simple factory is not a _real_ pattern, it is a **pseudo-pattern**
- a class that, based on a given input, returns objects of different types.
- The simple factory is just the **concrete factory**, without the extra factory class abstraction found in either the factory method or abstract factory.

#### Pros
- **encapsulates** object creation.
- **decouples** client code from object creation.
- **isolates** object creation logic, thus changes are made in only one place.

#### Cons
- if a new type of object is added to the system, then the logic to identify and create objects must be changed

*example - using the `LaptopFactory` concrete factory class to create the objects it needs via the `createLaptop()` method.*

![Simple Factory Class Diagram](https://dragonprogrammer.com/wp-content/uploads/2017/12/simple_factory_diagram_laptop.png)

- object creation is done in `LaptopFactory` based on information provided by the client code

```java
public class LaptopFactory {

    // Objects are created here 
    public Laptop createLaptop(String laptopType){
        if (laptopType == null) {
            return null;
        }

        switch (laptopType.toUpperCase()) {
            case "NormalLaptop":
                return new NormalLaptop();
                break;
            case "GamingLaptop":
                return new GamingLaptop();
                break;
                
            default:
                return null;
        }
    }
}
```

- client code receives an object depending on the requested type

```java
public class Client {
    public static void main(String[] args) {
        LaptopFactory laptopFactory = new LaptopFactory();

        // Get object of type GamingLaptop and run tests.
        Laptop myLaptop = laptopFactory.createLaptop("GamingLaptop");

        myLaptop.runTests();
    }
}
```

### Builder

#### 1. Introduction
- **Purpose**: Simplifies the creation of objects with numerous properties, preventing cluttered constructors and enhancing readability.
- **Definition**: A **creational design pattern** that separates object construction from representation, allowing for more flexible and maintainable object creation.

#### 2. Advantages of Builder Pattern
- **Flexibility**: Decouples construction from representation, enabling different object configurations without excessive constructors or setters.
- **Readability**: Uses fluent interfaces, making the object construction process intuitive and understandable for developers.
- **Immutability**: Facilitates the creation of immutable objects, ensuring thread safety and preventing accidental modifications after construction.

#### 3. Classic Builder Pattern
- create a separate _Builder_ inner class that contains methods to set each property of and build the constructed object
- facilitates a sequential configuration process and enhances code organization and readability

```java
public class Post {

    private final String title;

    private final String text;

    private final String category;

    Post(Builder builder) {
        this.title = builder.title;
        this.text = builder.text;
        this.category = builder.category;
    }

	/* getters */

    public static class Builder {
        private String title;
        private String text;
        private String category;

        public Builder title(String title) {
            this.title = title;
            return this;    // each method returns `this` allowing method chaining
        }

        public Builder text(String text) {
            this.text = text;
            return this;
        }

        public Builder category(String category) {
            this.category = category;
            return this;
        }

        public Post build() {
            return new Post(this);
        }
    }
}
```

- use the _Builder_ to create a new object:

```java
Post post = new Post.Builder()
  .title("Java Builder Pattern")
  .text("Explaining how to implement the Builder Pattern in Java")
  .category("Programming")
  .build();
```

#### 4. Generic Builder Pattern
- _GenericBuilder_ class can construct various types of objects by leveraging generics:

```java
public class GenericBuilder<T> {
    private final Supplier<T> supplier;

    private GenericBuilder(Supplier<T> supplier) {
        this.supplier = supplier;
    }

    public static <T> GenericBuilder<T> of(Supplier<T> supplier) {
        return new GenericBuilder<>(supplier);
    }

    public <P> GenericBuilder<T> with(BiConsumer<T, P> consumer, P value) {
        return new GenericBuilder<>(() -> {
            T object = supplier.get();
            consumer.accept(object, value);
            return object;
        });
    }

    public T build() {
        return supplier.get();
    }
}
```

- offers flexibility and readability, constructing every object concisely while ensuring type safety
- drawback: based on class setters, losing the immutability offered by the Builder Pattern

- create a new _GenericPost_ class consisting of a default no-args constructor, getters, and setters:

```
public class GenericPost {

    private String title;

    private String text;

    private String category;

    // getters and setters

}
```

- use our _GenericBuilder_ to create a _GenericPost_:

```
Post post = GenericBuilder.of(GenericPost::new)
  .with(GenericPost::setTitle, "Java Builder Pattern")
  .with(GenericPost::setText, "Explaining how to implement the Builder Pattern in Java")
  .with(GenericPost::setCategory, "Programming")
  .build();
```

#### 5. Lombok Builder
- [Lombok](https://www.baeldung.com/intro-to-project-lombok) is a library that simplifies Java code by automatically generating common methods
- By annotating a class with _@Builder_, Lombok generates a builder class with fluent methods for setting properties

- To use Lombok, import the dependency from the [Maven central repository](https://mvnrepository.com/artifact/org.projectlombok/lombok):

```java
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.32</version>
</dependency>
```

- Create a new _LombokPost_ class using the _@Builder_ annotation:

```java
@Builder
@Getter
public class LombokPost {
    private String title;
    private String text;
    private String category;
}
```

- use the builder pattern out of the box to create new objects:

```java
LombokPost lombokPost = LombokPost.builder()
  .title("Java Builder Pattern")
  .text("Explaining how to implement the Builder Pattern in Java")
  .category("Programming")
  .build();
```

### Factory Method
- provides an interface for creating objects in a superclass, but allows subclasses to alter the type of objects that will be created.
- **Approach**: Replace direct object creation (`new`) with a special _factory method_, creating objects within this method.
- **Flexibility**: Subclasses can modify the type of products returned by overriding the factory method, provided the products **share** a base class or interface.
- **Use Cases**
	- **Unknown Object Types**: Use when exact object types and dependencies are unknown at compile time.
	- **Extending Libraries/Frameworks**: Allowing external users to extend internal components of a framework.
	- **Resource Reuse**: Saves resources by reusing objects like database connections or network resources, managed in a central factory method.

####  Structure

![The structure of the Factory Method pattern](https://refactoring.guru/images/patterns/diagrams/factory-method/structure-indexed.png?id=4c603207859ca1f939b17b60a3a2e9e0)
1. **Product**: Declares the interface for all objects produced by the creator or its subclasses.
2. **Concrete Products**: Different implementations of the product interface.
3. **Creator**: Declares a factory method to return product objects. This method’s return type must match the product interface.
4. **Concrete Creators**: Override the base factory method to return specific product types.

- **Usage Note**: The factory method can either create new instances or return cached instances, providing flexibility in object management.

####  Pseudocode Example
- **Factory Method** used for creating cross-platform UI elements without coupling the client code to concrete UI classes.

![The structure of the Factory Method pattern example](https://refactoring.guru/images/patterns/diagrams/factory-method/example.png?id=67db9a5cb817913444efcb1c067c9835)

```java
// The creator class declares the factory method that must
// return an object of a product class. The creator's subclasses
// usually provide the implementation of this method.
class Dialog is
    // The creator may also provide some default implementation
    // of the factory method.
    abstract method createButton():Button

    // Note that, despite its name, the creator's primary
    // responsibility isn't creating products. It usually
    // contains some core business logic that relies on product
    // objects returned by the factory method. Subclasses can
    // indirectly change that business logic by overriding the
    // factory method and returning a different type of product
    // from it.
    method render() is
        // Call the factory method to create a product object.
        Button okButton = createButton()
        // Now use the product.
        okButton.onClick(closeDialog)
        okButton.render()

// Concrete creators override the factory method to change the
// resulting product's type.
class WindowsDialog extends Dialog is
    method createButton():Button is
        return new WindowsButton()

class WebDialog extends Dialog is
    method createButton():Button is
        return new HTMLButton()

// The product interface declares the operations that all
// concrete products must implement.
interface Button is
    method render()
    method onClick(f)

// Concrete products provide various implementations of the
// product interface.
class WindowsButton implements Button is
    method render(a, b) is
        // Render a button in Windows style.
    method onClick(f) is
        // Bind a native OS click event.

class HTMLButton implements Button is
    method render(a, b) is
        // Return an HTML representation of a button.
    method onClick(f) is
        // Bind a web browser click event.

class Application is
    field dialog: Dialog

    // The application picks a creator's type depending on the
    // current configuration or environment settings.
    method initialize() is
        config = readApplicationConfigFile()

        if (config.OS == "Windows") then
            dialog = new WindowsDialog()
        else if (config.OS == "Web") then
            dialog = new WebDialog()
        else
            throw new Exception("Error! Unknown operating system.")

    // The client code works with an instance of a concrete
    // creator, albeit through its base interface. As long as
    // the client keeps working with the creator via the base
    // interface, you can pass it any creator's subclass.
    method main() is
        this.initialize()
        dialog.render()
```

####  How to Implement
1. **Common Interface**: Define a common interface for all products.
2. **Empty Factory Method**: Add a factory method in the creator class with the product interface as its return type.
3. **Refactor Creation Code**: Replace direct product instantiation with factory method calls.
4. **Subclass Factories**: Create subclasses for each product type, overriding the factory method as needed.
5. **Optional Control Parameter**: If needed, add parameters to control product type selection within the factory.

####  Pros and Cons
**Pros**:
- **Decoupling**: Reduces tight coupling between creator and specific products.
- **Single Responsibility**: Centralizes creation code, simplifying support.
- **Open/Closed Principle**: Allows new product types without modifying client code.
**Cons**:
- **Complexity**: Requires additional subclasses, increasing complexity.

### Abstract Factory
- allows creating families of related objects without defining their concrete classes.
- **Declare Interfaces** for each product type to define consistent product behavior.
- **Define Abstract Factory** with creation methods for each product.
- **Create Concrete Factories** for each style variant (e.g., `ModernFurnitureFactory`, `VictorianFurnitureFactory`).
- **Client Code** works only with abstract interfaces, making it agnostic to specific product variants.
- **Initialize Concrete Factory** based on configuration or environment settings, allowing the app to dynamically select and create product variants.
- **Use Cases**:
	- When needing compatibility across families of related products.
	- When future extensibility is required without concrete class dependency.
	- When a class has multiple factory methods blurring its responsibility.

####  Structure
![Abstract Factory design pattern](https://refactoring.guru/images/patterns/diagrams/abstract-factory/structure-indexed.png?id=6ae1c99cbd90cf58753c633624fb1a04)
- **Abstract Products**: Interfaces defining the product family
- **Concrete Products**: Implementations of abstract products, each grouped by variant.
- **Abstract Factory Interface**: Declares methods to create each product.
- **Concrete Factories**: Implement creation methods, each producing a specific variant of products.
- **Client**: Uses factories and products via abstract interfaces, ensuring compatibility across product families without knowing concrete classes.

####  Pseudocode Example
- **Abstract Factory** pattern used for creating cross-platform UI elements without coupling the client code to concrete UI classes, while keeping all created elements consistent with a selected operating system.
![The class diagram for the Abstract Factory pattern example](https://refactoring.guru/images/patterns/diagrams/abstract-factory/example.png?id=5928a61d18bf00b047463471c599100a)

```java
// The abstract factory interface declares a set of methods that
// return different abstract products. These products are called
// a family and are related by a high-level theme or concept.
// Products of one family are usually able to collaborate among
// themselves. A family of products may have several variants,
// but the products of one variant are incompatible with the
// products of another variant.
interface GUIFactory is
    method createButton():Button
    method createCheckbox():Checkbox

// Concrete factories produce a family of products that belong
// to a single variant. The factory guarantees that the
// resulting products are compatible. Signatures of the concrete
// factory's methods return an abstract product, while inside
// the method a concrete product is instantiated.
class WinFactory implements GUIFactory is
    method createButton():Button is
        return new WinButton()
    method createCheckbox():Checkbox is
        return new WinCheckbox()

// Each concrete factory has a corresponding product variant.
class MacFactory implements GUIFactory is
    method createButton():Button is
        return new MacButton()
    method createCheckbox():Checkbox is
        return new MacCheckbox()

// Each distinct product of a product family should have a base
// interface. All variants of the product must implement this
// interface.
interface Button is
    method paint()

// Concrete products are created by corresponding concrete
// factories.
class WinButton implements Button is
    method paint() is
        // Render a button in Windows style.

class MacButton implements Button is
    method paint() is
        // Render a button in macOS style.

// Here's the base interface of another product. All products
// can interact with each other, but proper interaction is
// possible only between products of the same concrete variant.
interface Checkbox is
    method paint()

class WinCheckbox implements Checkbox is
    method paint() is
        // Render a checkbox in Windows style.

class MacCheckbox implements Checkbox is
    method paint() is
        // Render a checkbox in macOS style.

// The client code works with factories and products only
// through abstract types: GUIFactory, Button and Checkbox. This
// lets you pass any factory or product subclass to the client
// code without breaking it.
class Application is
    private field factory: GUIFactory
    private field button: Button
    constructor Application(factory: GUIFactory) is
        this.factory = factory
    method createUI() is
        this.button = factory.createButton()
    method paint() is
        button.paint()

// The application picks the factory type depending on the
// current configuration or environment settings and creates it
// at runtime (usually at the initialization stage).
class ApplicationConfigurator is
    method main() is
        config = readApplicationConfigFile()

        if (config.OS == "Windows") then
            factory = new WinFactory()
        else if (config.OS == "Mac") then
            factory = new MacFactory()
        else
            throw new Exception("Error! Unknown operating system.")

        Application app = new Application(factory)
```

####  How to Implement
1. Map out product types and variants.
2. Declare abstract interfaces for each product.
3. Implement concrete classes for each product variant.
4. Define an abstract factory interface with creation methods.
5. Implement concrete factories for each variant.
6. Replace direct product instantiation with factory methods in the client code.

####  Pros and Cons
**Pros**
- Ensures compatibility between created products.
- Reduces coupling between client and concrete classes.
- Supports **Single Responsibility** and **Open/Closed** principles.
- Centralizes product creation for easier maintenance.
**Cons**
- Increased complexity due to additional interfaces and classes.

## Behavior Patterns

### Iterator
- Allows traversing elements of a collection without exposing its underlying structure (e.g., list, stack, tree).
- **Iterator Object**: Encapsulates traversal logic, including position and remaining elements, enabling independent traversal across multiple iterators.
- **Single Interface**: Standardizes traversal across collections; clients interact through the iterator interface, not the collection type.
- **New Iterators**: Create new iterator classes as needed without modifying collection or client code.
- **Use Cases**:
	- **Complex Structures**: Use when the collection has an underlying complex structure hidden from the client.
	- **Code Simplification**: Reduce duplicate traversal code, enhancing maintainability by delegating traversal logic to iterators.
	- **Diverse Structures**: Supports traversal of various collection types using common interfaces.

####  Structure

![Structure of the Iterator design pattern](https://refactoring.guru/images/patterns/diagrams/iterator/structure-indexed.png?id=7bc28907ff6b480db6635a93ebaa10ff)
- **Iterator Interface**: Declares operations for traversing a collection (e.g., next element, current position).
- **Concrete Iterator**: Implements specific traversal algorithms and tracks traversal progress.
- **Collection Interface**: Provides methods to obtain compatible iterators for traversing its elements.
- **Concrete Collection**: Creates instances of iterators tailored for its structure, allowing various client interactions without direct exposure.
- **Client**: Works with iterators via interfaces, supporting various collections and traversal methods.

####  Pseudocode Example
- **Iterator** pattern is used to walk through a special kind of collection which encapsulates access to Facebook’s social graph

![Structure of the Iterator pattern example](https://refactoring.guru/images/patterns/diagrams/iterator/example.png?id=f2a24ef3787bf80ed450709240506ff2)

```java
// The collection interface must declare a factory method for
// producing iterators. You can declare several methods if there
// are different kinds of iteration available in your program.
interface SocialNetwork is
    method createFriendsIterator(profileId):ProfileIterator
    method createCoworkersIterator(profileId):ProfileIterator

// Each concrete collection is coupled to a set of concrete
// iterator classes it returns. But the client isn't, since the
// signature of these methods returns iterator interfaces.
class Facebook implements SocialNetwork is
    // ... The bulk of the collection's code should go here ...

    // Iterator creation code.
    method createFriendsIterator(profileId) is
        return new FacebookIterator(this, profileId, "friends")
    method createCoworkersIterator(profileId) is
        return new FacebookIterator(this, profileId, "coworkers")

// The common interface for all iterators.
interface ProfileIterator is
    method getNext():Profile
    method hasMore():bool

// The concrete iterator class.
class FacebookIterator implements ProfileIterator is
    // The iterator needs a reference to the collection that it
    // traverses.
    private field facebook: Facebook
    private field profileId, type: string

    // An iterator object traverses the collection independently
    // from other iterators. Therefore it has to store the
    // iteration state.
    private field currentPosition
    private field cache: array of Profile

    constructor FacebookIterator(facebook, profileId, type) is
        this.facebook = facebook
        this.profileId = profileId
        this.type = type

    private method lazyInit() is
        if (cache == null)
            cache = facebook.socialGraphRequest(profileId, type)

    // Each concrete iterator class has its own implementation
    // of the common iterator interface.
    method getNext() is
        if (hasMore())
            result = cache[currentPosition]
            currentPosition++
            return result

    method hasMore() is
        lazyInit()
        return currentPosition < cache.length

// Here is another useful trick: you can pass an iterator to a
// client class instead of giving it access to a whole
// collection. This way, you don't expose the collection to the
// client.
//
// And there's another benefit: you can change the way the
// client works with the collection at runtime by passing it a
// different iterator. This is possible because the client code
// isn't coupled to concrete iterator classes.
class SocialSpammer is
    method send(iterator: ProfileIterator, message: string) is
        while (iterator.hasMore())
            profile = iterator.getNext()
            System.sendEmail(profile.getEmail(), message)

// The application class configures collections and iterators
// and then passes them to the client code.
class Application is
    field network: SocialNetwork
    field spammer: SocialSpammer

    method config() is
        if working with Facebook
            this.network = new Facebook()
        if working with LinkedIn
            this.network = new LinkedIn()
        this.spammer = new SocialSpammer()

    method sendSpamToFriends(profile) is
        iterator = network.createFriendsIterator(profile.getId())
        spammer.send(iterator, "Very important message")

    method sendSpamToCoworkers(profile) is
        iterator = network.createCoworkersIterator(profile.getId())
        spammer.send(iterator, "Very important message")
```

####  How to Implement
- **Iterator Interface**: Define methods for fetching elements, position tracking, and iteration end-check.
- **Collection Interface**: Define methods to fetch iterators; tailor iterators based on traversal needs.
- **Concrete Iterator Classes**: Link iterator to a specific collection; establish connection via the iterator’s constructor.
- **Client Code**: Replace direct collection traversal with iterators.

####  Pros and Cons

##### Pros
- **Single Responsibility**: Cleaner code, extracting traversal from collections and clients.
- **Open/Closed**: New collections/iterators integrate without breaking existing code.
- **Parallel Iteration**: Each iterator holds its own state, enabling parallel traversal.
- **Delayed Iteration**: Iteration can pause and resume as needed.

##### Cons
- **Overhead for Simple Collections**: Pattern may add unnecessary complexity.
- **Performance**: Direct traversal might be faster in certain specialized collections.

### Strategy
- allows defining a family of algorithms, each encapsulated in its own class, making them interchangeable at runtime.
- suggests taking a class that does something specific in many different ways and extracting each algorithm into separate classes called **strategies**
- the original class, called **context**, stores a reference to a specific strategy and delegates tasks to it
- the client passes the desired strategy to the context so that the context is independent from the strategies
- **Use Cases**:
	- Different algorithm versions are needed, with runtime flexibility for switching.
	- Similar classes vary only in specific behaviors, allowing simplification through shared interfaces.
	- Large conditional statements switching between algorithms need simplification.

####  Structure

![Structure of the Strategy design pattern](https://refactoring.guru/images/patterns/diagrams/strategy/structure-indexed.png?id=ff55c5a6273cf82a5667f3cab5b605c7)
- **Context**: Holds a reference to a strategy and interacts with it via a common interface.
- **Strategy Interface**: Declares the method all strategies must implement.
- **Concrete Strategies**: Different classes implementing the strategy interface with specific algorithms.
- **Client**: Instantiates a strategy and sets it in the context, allowing for runtime changes.

####  Pseudocode Example
- use multiple **strategies** to execute various arithmetic operations.

```java
// The strategy interface declares operations common to all
// supported versions of some algorithm. The context uses this
// interface to call the algorithm defined by the concrete
// strategies.
interface Strategy is
    method execute(a, b)

// Concrete strategies implement the algorithm while following
// the base strategy interface. The interface makes them
// interchangeable in the context.
class ConcreteStrategyAdd implements Strategy is
    method execute(a, b) is
        return a + b

class ConcreteStrategySubtract implements Strategy is
    method execute(a, b) is
        return a - b

class ConcreteStrategyMultiply implements Strategy is
    method execute(a, b) is
        return a * b

// The context defines the interface of interest to clients.
class Context is
    // The context maintains a reference to one of the strategy
    // objects. The context doesn't know the concrete class of a
    // strategy. It should work with all strategies via the
    // strategy interface.
    private strategy: Strategy

    // Usually the context accepts a strategy through the
    // constructor, and also provides a setter so that the
    // strategy can be switched at runtime.
    method setStrategy(Strategy strategy) is
        this.strategy = strategy

    // The context delegates some work to the strategy object
    // instead of implementing multiple versions of the
    // algorithm on its own.
    method executeStrategy(int a, int b) is
        return strategy.execute(a, b)

// The client code picks a concrete strategy and passes it to
// the context. The client should be aware of the differences
// between strategies in order to make the right choice.
class ExampleApplication is
    method main() is
        Create context object.

        Read first number.
        Read last number.
        Read the desired action from user input.

        if (action == addition) then
            context.setStrategy(new ConcreteStrategyAdd())

        if (action == subtraction) then
            context.setStrategy(new ConcreteStrategySubtract())

        if (action == multiplication) then
            context.setStrategy(new ConcreteStrategyMultiply())

        result = context.executeStrategy(First number, Second number)

        Print result.
```

####  How to Implement
1. Identify the algorithm subject to frequent changes.
2. Create a common strategy interface for the algorithms.
3. Extract each algorithm to its own class, implementing the strategy interface.
4. In the context class, add a reference to a strategy object and provide a setter for switching.
5. Let clients associate the context with a suitable strategy.

####  Pros and Cons
- **Pros**:
    - Algorithms are interchangeable at runtime.
    - Isolates algorithm implementation from usage.
    - Promotes composition over inheritance, supporting the Open/Closed Principle.
- **Cons**:
    - Extra classes and interfaces may overcomplicate the design for few or infrequent algorithms.
    - Clients need awareness of strategy differences for proper selection.
    - Functional programming languages offer anonymous functions, which can replace strategy objects without extra classes.

### Observer
- enables a subscription mechanism, notifying multiple objects (subscribers) about changes in the state of another object (publisher)
- **Use Cases**:
	- When object state changes require updates in other objects.
	- When interested objects are unknown or change dynamically (e.g., GUI buttons).
	- When objects need temporary observation in specific cases.
- **Mechanism**:
    - **Subscription List**: An array in the publisher holds references to subscriber objects.
    - **Methods**: Public methods add/remove subscribers from this list.

####  Structure

![Structure of the Observer design pattern](https://refactoring.guru/images/patterns/diagrams/observer/structure-indexed.png?id=2ca2c123503ede860740af2a22bc4b4d)
- **Publisher**:
    - Issues events when its state changes.
    - Manages subscribers and notifies them of updates by calling a method declared in the subscriber interface.
- **Subscriber Interface**:
    - Declares the notification method (`update`) that publisher calls.
    - Allows context data to be passed, such as the publisher itself, for event-specific information.
- **Concrete Subscribers**:
    - Implement the subscriber interface and handle notifications based on event context.

####  Pseudocode Example
- **Observer** pattern lets the text editor object notify other service objects about changes in its state

![Structure of the Observer pattern example](https://refactoring.guru/images/patterns/diagrams/observer/example.png?id=6d0603ab5a00e4463b81d9639cd746a2)

```java
// The base publisher class includes subscription management
// code and notification methods.
class EventManager is
    private field listeners: hash map of event types and listeners

    method subscribe(eventType, listener) is
        listeners.add(eventType, listener)

    method unsubscribe(eventType, listener) is
        listeners.remove(eventType, listener)

    method notify(eventType, data) is
        foreach (listener in listeners.of(eventType)) do
            listener.update(data)

// The concrete publisher contains real business logic that's
// interesting for some subscribers. We could derive this class
// from the base publisher, but that isn't always possible in
// real life because the concrete publisher might already be a
// subclass. In this case, you can patch the subscription logic
// in with composition, as we did here.
class Editor is
    public field events: EventManager
    private field file: File

    constructor Editor() is
        events = new EventManager()

    // Methods of business logic can notify subscribers about
    // changes.
    method openFile(path) is
        this.file = new File(path)
        events.notify("open", file.name)

    method saveFile() is
        file.write()
        events.notify("save", file.name)

    // ...

// Here's the subscriber interface. If your programming language
// supports functional types, you can replace the whole
// subscriber hierarchy with a set of functions.
interface EventListener is
    method update(filename)

// Concrete subscribers react to updates issued by the publisher
// they are attached to.
class LoggingListener implements EventListener is
    private field log: File
    private field message: string

    constructor LoggingListener(log_filename, message) is
        this.log = new File(log_filename)
        this.message = message

    method update(filename) is
        log.write(replace('%s',filename,message))

class EmailAlertsListener implements EventListener is
    private field email: string
    private field message: string

    constructor EmailAlertsListener(email, message) is
        this.email = email
        this.message = message

    method update(filename) is
        system.email(email, replace('%s',filename,message))

// An application can configure publishers and subscribers at
// runtime.
class Application is
    method config() is
        editor = new Editor()

        logger = new LoggingListener(
            "/path/to/log.txt",
            "Someone has opened the file: %s")
        editor.events.subscribe("open", logger)

        emailAlerts = new EmailAlertsListener(
            "admin@example.com",
            "Someone has changed the file: %s")
        editor.events.subscribe("save", emailAlerts)
```

####  How to Implement
1. **Define Publisher/Subscribers**:
    - Core functionality as the publisher; auxiliary functions as subscribers.
2. **Declare Interfaces**:
    - Subscriber with `update` method.
    - Publisher with add/remove subscriber methods.
3. **Subscription Logic**:
    - Use an abstract class for shared subscription behavior.
    - Alternatively, use composition for existing hierarchies.
4. **Notification Mechanism**:
    - Pass context data (e.g., event info) in `update` method.
5. **Client Setup**:
    - Create subscribers and register them with publishers.

####  Pros and Cons
- **Pros**:
    - Adheres to the **Open/Closed Principle**: New subscribers can be added without modifying publisher code.
    - Establishes dynamic runtime relationships between objects.
- **Cons**:
    - **Notification Order**: Subscribers are notified in a random order.

## Structural Patterns

### Adaptor
- **Adapter Creation**: An adapter converts one interface to a compatible one, wrapping the incompatible object and managing the conversion.
- **Use Cases**:
	- adapt a service with an incompatible interface to be compatible with existing client code.
	- reuse subclasses lacking some common functionality (without changing the original classes).
- **Mechanics**:
    - Adapter implements the client-compatible interface, handling the conversions needed for the target object.
    - Communication flows through the adapter, ensuring the incompatible object remains unaware of any changes.
- **Two-way adapters**: Enable conversion in both directions if needed.

####  Structure

##### Object adapter
- uses the object composition principle: the adapter implements the interface of one object and wraps the other one
![Structure of the Adapter design pattern (the object adapter)](https://refactoring.guru/images/patterns/diagrams/adapter/structure-object-adapter-indexed.png?id=a20b311948b361a058097e5bcdbf067a)
- adapter receives calls from the client via the client interface and translates them into calls to the wrapped service object in a format it can understand.
- if the interface of the service class gets changed or replaced, a new adapter class can be created without changing the client code.

##### Class adapter
- uses inheritance: the adapter inherits interfaces from both objects at the same time
![Adapter design pattern (class adapter)](https://refactoring.guru/images/patterns/diagrams/adapter/structure-class-adapter-indexed.png?id=250b5c485a7dfba7c16b89a9201538fb)
- adaptation happens within the overridden methods
- adapter can be used in place of an existing client class

####  Pseudocode Example
![Structure of the Adapter pattern example](https://refactoring.guru/images/patterns/diagrams/adapter/example.png?id=9d2b6857ce256f2c669383ce4df3d0aa)

```java
// Say you have two classes with compatible interfaces:
// RoundHole and RoundPeg.
class RoundHole is
    constructor RoundHole(radius) { ... }

    method getRadius() is
        // Return the radius of the hole.

    method fits(peg: RoundPeg) is
        return this.getRadius() >= peg.getRadius()

class RoundPeg is
    constructor RoundPeg(radius) { ... }

    method getRadius() is
        // Return the radius of the peg.

// But there's an incompatible class: SquarePeg.
class SquarePeg is
    constructor SquarePeg(width) { ... }

    method getWidth() is
        // Return the square peg width.

// An adapter class lets you fit square pegs into round holes.
// It extends the RoundPeg class to let the adapter objects act
// as round pegs.
class SquarePegAdapter extends RoundPeg is
    // In reality, the adapter contains an instance of the
    // SquarePeg class.
    private field peg: SquarePeg

    constructor SquarePegAdapter(peg: SquarePeg) is
        this.peg = peg

    method getRadius() is
        // The adapter pretends that it's a round peg with a
        // radius that could fit the square peg that the adapter
        // actually wraps.
        return peg.getWidth() * Math.sqrt(2) / 2

// Somewhere in client code.
hole = new RoundHole(5)
rpeg = new RoundPeg(5)
hole.fits(rpeg) // true

small_sqpeg = new SquarePeg(5)
large_sqpeg = new SquarePeg(10)
hole.fits(small_sqpeg) // this won't compile (incompatible types)

small_sqpeg_adapter = new SquarePegAdapter(small_sqpeg)
large_sqpeg_adapter = new SquarePegAdapter(large_sqpeg)
hole.fits(small_sqpeg_adapter) // true
hole.fits(large_sqpeg_adapter) // false
```

####  How to Implement
- Identify two incompatible classes.
- Create a **client interface** defining how clients interact with the service.
- Create an **adapter class** implementing the client interface, handling data/interface conversion as needed.
- **Implement methods** that translate client calls to the service format.

####  Pros and Cons
- **Pros**:
    - **Single Responsibility Principle**: Separates conversion logic from core business functions.
    - **Open/Closed Principle**: New adapters can be added without impacting client code.
- **Cons**:
    - Increases code complexity by introducing additional interfaces and classes.

### Facade
- simplified interface to a library, a framework, or any other complex set of classes.
- provides only essential functionality, hiding unnecessary details.
- **Use Cases**:
	- When you need a straightforward interface for a complex subsystem.
	- When subsystems grow complex over time, requiring extensive setup or boilerplate code.
	- To structure subsystems in layers, creating facades as entry points for each layer, reducing coupling.

####  Structure

![Structure of the Facade design pattern](https://refactoring.guru/images/patterns/diagrams/facade/structure-indexed.png?id=2da06d6b850701ea15cf72f9d2642fb8)
- **Facade**: Provides access to parts of a subsystem’s functionality and directs client requests efficiently.
- **Additional Facades**: Can separate unrelated features, avoiding a bloated facade.
- **Complex Subsystem**: Contains numerous objects requiring precise interaction; unaware of the facade.
- **Client**: Interacts only with the facade, not the subsystem directly.

####  Pseudocode Example
- **Facade** pattern simplifies interaction with a complex video conversion framework.

![The structure of the Facade pattern example](https://refactoring.guru/images/patterns/diagrams/facade/example.png?id=2249d134e3ff83819dfc19032f02eced)

```java
// These are some of the classes of a complex 3rd-party video
// conversion framework. We don't control that code, therefore
// can't simplify it.

class VideoFile
// ...

class OggCompressionCodec
// ...

class MPEG4CompressionCodec
// ...

class CodecFactory
// ...

class BitrateReader
// ...

class AudioMixer
// ...

// We create a facade class to hide the framework's complexity
// behind a simple interface. It's a trade-off between
// functionality and simplicity.
class VideoConverter is
    method convert(filename, format):File is
        file = new VideoFile(filename)
        sourceCodec = (new CodecFactory).extract(file)
        if (format == "mp4")
            destinationCodec = new MPEG4CompressionCodec()
        else
            destinationCodec = new OggCompressionCodec()
        buffer = BitrateReader.read(filename, sourceCodec)
        result = BitrateReader.convert(buffer, destinationCodec)
        result = (new AudioMixer()).fix(result)
        return new File(result)

// Application classes don't depend on a billion classes
// provided by the complex framework. Also, if you decide to
// switch frameworks, you only need to rewrite the facade class.
class Application is
    method main() is
        convertor = new VideoConverter()
        mp4 = convertor.convert("funny-cats-video.ogg", "mp4")
        mp4.save()
```

####  How to Implement
1. Check if the subsystem can be simplified.
2. Create a facade class to redirect client requests to the appropriate subsystem objects.
3. Ensure all client interactions go through the facade.
4. If the facade becomes complex, extract parts of its behavior into additional facades.

####  Pros and Cons
**Pros**:
    - Simplifies the interaction with complex subsystems.
    - Reduces dependencies between client code and subsystem details.
**Cons**:
    - A facade can become a “god object,” tightly coupled to many classes.

### Decorator
- Attach new behaviors to objects by placing these objects inside special wrapper objects that contain the behaviors
- Use _aggregation_ or _composition_ over inheritance.
- Create "wrapper" decorators to add behaviors dynamically at runtime
- **Wrapper Functionality**: Decorators "wrap" objects, implementing the same interface, and can modify requests passed to the target.
- **Layering Behavior**: Multiple decorators can wrap an object, layering behaviors and modifying outcomes based on decorator sequence.
- **Use Cases**
	- Ideal when adding behaviors at runtime without modifying existing code.
	- Useful for extending `final` classes or when behavior modification via subclassing is impractical.

####  Structure

![Structure of the Decorator design pattern](https://refactoring.guru/images/patterns/diagrams/decorator/structure-indexed.png?id=09401b230a58f2249e4c9a1195d485a0)
- **Component Interface**: Defines methods common to both wrappers and wrapped objects.
- **Concrete Component**: Base class defining the core behavior.
- **Base Decorator**: References the component interface and delegates work to it.
- **Concrete Decorators**: Extend the base decorator, implementing additional behaviors by overriding methods.
- **Client**: Wraps components in multiple decorators, utilizing their combined behaviors.

####  Pseudocode Example
- **Decorator** pattern used to compress and encrypt sensitive data independently from the code that actually uses this data.

![Structure of the Decorator pattern example](https://refactoring.guru/images/patterns/diagrams/decorator/example.png?id=eec9dc488f00c85f50e764323baa723e)

```java
// The component interface defines operations that can be
// altered by decorators.
interface DataSource is
    method writeData(data)
    method readData():data

// Concrete components provide default implementations for the
// operations. There might be several variations of these
// classes in a program.
class FileDataSource implements DataSource is
    constructor FileDataSource(filename) { ... }

    method writeData(data) is
        // Write data to file.

    method readData():data is
        // Read data from file.

// The base decorator class follows the same interface as the
// other components. The primary purpose of this class is to
// define the wrapping interface for all concrete decorators.
// The default implementation of the wrapping code might include
// a field for storing a wrapped component and the means to
// initialize it.
class DataSourceDecorator implements DataSource is
    protected field wrappee: DataSource

    constructor DataSourceDecorator(source: DataSource) is
        wrappee = source

    // The base decorator simply delegates all work to the
    // wrapped component. Extra behaviors can be added in
    // concrete decorators.
    method writeData(data) is
        wrappee.writeData(data)

    // Concrete decorators may call the parent implementation of
    // the operation instead of calling the wrapped object
    // directly. This approach simplifies extension of decorator
    // classes.
    method readData():data is
        return wrappee.readData()

// Concrete decorators must call methods on the wrapped object,
// but may add something of their own to the result. Decorators
// can execute the added behavior either before or after the
// call to a wrapped object.
class EncryptionDecorator extends DataSourceDecorator is
    method writeData(data) is
        // 1. Encrypt passed data.
        // 2. Pass encrypted data to the wrappee's writeData
        // method.

    method readData():data is
        // 1. Get data from the wrappee's readData method.
        // 2. Try to decrypt it if it's encrypted.
        // 3. Return the result.

// You can wrap objects in several layers of decorators.
class CompressionDecorator extends DataSourceDecorator is
    method writeData(data) is
        // 1. Compress passed data.
        // 2. Pass compressed data to the wrappee's writeData
        // method.

    method readData():data is
        // 1. Get data from the wrappee's readData method.
        // 2. Try to decompress it if it's compressed.
        // 3. Return the result.

// Option 1. A simple example of a decorator assembly.
class Application is
    method dumbUsageExample() is
        source = new FileDataSource("somefile.dat")
        source.writeData(salaryRecords)
        // The target file has been written with plain data.

        source = new CompressionDecorator(source)
        source.writeData(salaryRecords)
        // The target file has been written with compressed
        // data.

        source = new EncryptionDecorator(source)
        // The source variable now contains this:
        // Encryption > Compression > FileDataSource
        source.writeData(salaryRecords)
        // The file has been written with compressed and
        // encrypted data.

// Option 2. Client code that uses an external data source.
// SalaryManager objects neither know nor care about data
// storage specifics. They work with a pre-configured data
// source received from the app configurator.
class SalaryManager is
    field source: DataSource

    constructor SalaryManager(source: DataSource) { ... }

    method load() is
        return source.readData()

    method save() is
        source.writeData(salaryRecords)
    // ...Other useful methods...

// The app can assemble different stacks of decorators at
// runtime, depending on the configuration or environment.
class ApplicationConfigurator is
    method configurationExample() is
        source = new FileDataSource("salary.dat")
        if (enabledEncryption)
            source = new EncryptionDecorator(source)
        if (enabledCompression)
            source = new CompressionDecorator(source)

        logger = new SalaryManager(source)
        salary = logger.load()
    // ...
```

####  How to Implement
1. **Identify Domain Layers**: Determine primary component and optional layers.
2. **Define Common Interface**: For primary and optional layers.
3. **Base Component Class**: Implements core behavior.
4. **Base Decorator Class**: Holds reference to the wrapped component and delegates functionality.
5. **Concrete Decorators**: Implement additional behaviors.
6. **Client Code**: Creates and composes decorators as needed.

####  Pros and Cons
**Pros**:
- Adds behaviors without subclassing.
- Enables runtime addition/removal of responsibilities.
- Allows behavior combinations by stacking multiple decorators.
- Adheres to the _Single Responsibility Principle_.

**Cons**:
- Removing specific wrappers from a stack is challenging.
- Decorator behavior order can impact functionality.
- Initial setup of decorator layers can appear complex.

# Factory Patterns

## 1. Factory
- A general term for functions, methods, or classes that create various entities (objects, files, records).

## 2. Creation method
- A method that creates objects using a wrapper around a constructor.

*example - `next` is a creation method:*

```java
class Number {
    private $value;

    public function __construct($value) {
        \( this->value =  \)value;
    }

    public function next() {
        return new Number ($this->value + 1);
    }
}
```

## 3. Static creation method
- creates and returns objects, often as an alternative constructor
- **Usage**:
    - When multiple constructors with similar signatures are needed.
    - To reuse existing objects (e.g., Singleton pattern).

*example - `load` method is a static creation method*

```java
class User {
    private \( id,  \)name, \( email,  \)phone;

    public function __construct(\( id,  \)name, \( email,  \)phone) {
        \( this->id =  \)id;
        \( this->name =  \)name;
        \( this->email =  \)email;
        \( this->phone =  \)phone;
    }

    public static function load($id) {
        list(\( id,  \)name, \( email,  \)phone) = DB::load_data('users', 'id', 'name', 'email', 'phone');
        \( user = new User( \)id, \( name,  \)email, $phone);
        return $user;
    }
}
```

## 4. _Simple factory_ pattern
- A single class with a creation method using conditionals to instantiate and return different objects based on parameters.

*example*

```java
class UserFactory {
    public static function create($type) {
        switch ($type) {
            case 'user': return new User();
            case 'customer': return new Customer();
            case 'admin': return new Admin();
            default:
                throw new Exception('Wrong user type passed.');
        }
    }
}
```

## 5. _Factory Method_ pattern
- A creational pattern that provides an interface for creating objects, allowing subclasses to modify the object type.
- Commonly used in base classes, with subclasses extending the creation logic.

```java
abstract class Department {
    public abstract function createEmployee($id);

    public function fire($id) {
        \( employee =  \)this->createEmployee($id);
        $employee->paySalary();
        $employee->dismiss();
    }
}

class ITDepartment extends Department {
    public function createEmployee($id) {
        return new Programmer($id);
    }
}

class AccountingDepartment extends Department {
    public function createEmployee($id) {
        return new Accountant($id);
    }
}
```

## 6. _Abstract Factory_ pattern
- A creational pattern that produces families of related objects without specifying their concrete classes.
- Works with "families of objects"
- example: variants of the set of classes `Transport` + `Engine` + `Controls`:
	1. `Car` + `CombustionEngine` + `SteeringWheel`
	2. `Plane` + `JetEngine` + `Yoke`