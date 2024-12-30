---
layout: default
title: Java
parent: Midterm Notes
nav_order: 2
---
# Java
## Compiling
- high-level source code must be compiled into an executable stored in bytes for the computer to run
- advantage of .exe for C: can run executable without any tools
- disadvantage of .exe: executable only works on original OS
- Java built to address limit of cross-compiling
- Python is an interpreted language → compiling and running is done at the same time
- interpreted languages usually less efficient than compiled languages
## How Java works

### JDK
- JDK compiles .java file and creates .class file
- .class file is **portable** between environments, unlike C
- .class files run in virtual Java Runtime Environment
![An image showing a .java file being compiled into a .class file which is being used by a box labeled Java Runtime Environment, which is running on the computer](https://sde-coursepack.github.io/modules/java/images/3/compiler3.png)
### JRE
- JRE included with JDK download, or can be downloaded separately
- contain core class libraries but can be customized for specialized runtime environments
### JVM
- JRE contains Java Virtual Machine which handles direct interactions with host hardware
- JVM interprets .class byte-code
### JIT
- Just In Time complier is a part of JVM that compiles JVM byte-code into native machine code
- more efficient than interpreting, can be further optimized
## Why Java is built this way
- compiling with JDK allows sharing of executables across different OS that can be run with JRE/JVM
- JIT allows for optimization
- overall convenience, distributability, portability, and performance
## JVM: It’s not just for Java anymore
- JRE can run any compatible .class byte-code, such as code written in Kotlin and Groovy
- Kotlin is popular in Android community
- Groovy is used is Gradle as a domain-specific language (DSL)
## Key takeaways
- Compiling is taking a source code resource, and producing a bytecode resource designed to run on a particular machine
- Interpreting is similar to compiling, but is done dynamically at runtime
- Java uses a JDK for compiling in bytecode that runs on a virtual machine
- The JRE runs Java programs via a JVM
- The JVM interprets java bytecode into machine instructions of the underlying hardware
- The JIT is used to compile the java bytecode into machine instructions for the underlying hardware at runtime, and boosts optimization and efficiency.
# Terminal
## Terminal
- `ls`: prints a list of the current directory contents
- `cd`: changes directory
- `Tab`: auto-complete
- `..`: parent folder
## Getting started with Java Commands
- `java --version`: check version of Java
- `javac`: compiles java file(s)
- files that are **dependencies** must be compiled first/same time
- `java`: runs a program
### .Jar files
- contains several files that can be run as a single Java class
- `jar cf [name].jar [folder]*.class`: creates a `.jar` file with the files in `[folder]`
- `jar cfe [name].jar [folder].Main [folder]/*.class`: specifies which file to run when running the `.jar` file, `e` for entry point
- `java -jar [name].jar`: runs a `.jar` file
# Packages

## Importing packages
### A note on import statements
- as a general rule, it’s fine to just use `import java.util.*` whenever you’re using anything in the java.util library
- only import individual classes in the case of a name conflict

```
import java.util.*;
import java.awt.*;
import java.util.List;
```

## Creating packages
- `package [name of package]`; package declaration at top of file creates a sub-folder in the source code director
### Sharing code
- classes that aren’t in a package use the **default namespace**
- using packages allows other users to import only what is relevant when sharing code to avoid name conflicts
### Design Considerations
- packages are useful for modularity and reusing code that has a clear purpose
## Naming packages
- when naming packages, use “reverse Internet domain name”
- ex. cs.virginia.edu becomes `edu.virginia.cs.[project-name]`
## Adding Jars as Libraries
- process for most IDEs: copy/paste library as `.jar` file, “Add as Library”
# Command Line Arguments
## main(String[] args)
- `String[] args` used for command-line arguments
- arguments are split by whitespace, `args[0]` is first argument after program name
### Why do we want these?
- allows user to input information without editing source code/using System.in
- important for program-to-program communication
## String Arguments
*example*

```Java
public class HelloWho{
    public static void main(String[] args) {
        try {
            String who = args[0];
            System.out.println("Hello, " + who);
        } catch (ArrayIndexOutOfBoundsException e) {
            System.out.println("Error: This program requires a command line argument for a name. format:\n" +
            "\t  java edu.virginia.cs.commandline.HelloWho name");
        }
    }
}
```

```shell
# running with one argument
java edu.virginia.cs.commandline.HelloWho Will
# will output
Hello, Will

# running with an argument with whitespace
java edu.virginia.cs.commandline.HelloWho "Will McBurney"
# will output
Hello, Will McBurney

# running without an argument
java edu.virginia.cs.commandline.HelloWho
# will output
Error: This program requires a command line argument for a name. format:
    java edu.virginia.cs.commandline.HelloWho name
```

## Numeric Arguments
*example*

```Java
public class HelloNTimes{
	    public static void main(String[] args) {
        try {
	        // use Interger.parseInt to convert String to int
            int count = Integer.parseInt(args[0]);
            printHelloNTimes(count);
        } catch (ArrayIndexOutOfBoundsException e) {
            System.out.println("Error: Must include a number argument. Example:\n"+
                    "\tjava edu.virginia.cs.commandline.HelloNTimes 5");
        } catch (NumberFormatException e) {
            System.out.println("Error: First argument was not an integer. Example:\n"+
                    "\tjava edu.virginia.cs.commandline.HelloNTimes 5");
        }
    }
}
```

```shell
# running with an int argument
java java edu.virginia.cs.commandline.HelloNTimes 5
# will output
Hello!
Hello!
Hello!
Hello!
Hello!

# running with improper type argument
java edu.virginia.cs.commandline.HelloNTimes Steve
# or
java edu.virginia.cs.commandline.HelloNTimes 3.6
# will output
Error: First argument was not an integer. Example:
    \tjava edu.virginia.cs.commandline.HelloNTimes 5
```

## Multiple Arguments
*example*

```Java
public class MyGCD{
	public static void main(String[] args) {
        int a = Integer.parseInt(args[0]);
        int b = Integer.parseInt(args[1]);
        int gcd = gcd(a,b);
        String toPrint = String.format("GCD(%d, %d) = %d", a, b, gcd);
        System.out.println(toPrint);
    }
}
```

```shell
# running
java edu.virginia.cs.commandline.MyGCD 144 81
# will output
GCD(144, 81) = 9
```

## Optional Arguments
*example*

```Java
    public static void main(String[] args){
	    // creates List from Array, includes method contains()
        List<String> argList= Arrays.asList(args);
        boolean isJulian=checkForJulianFlag(argList);
        ...
    }

	// if command includes -j or --julian *anywhere*, use Julian Calendar
    private static boolean checkForJulianFlag(List<String> argList) {
        return argList.contains("-j") || argList.contains("--julian");
    }
```

```shell
# running
java edu.virginia.cs.commandline.isLeapYear 1900
# will output
1900 is not a leap year

# running
java edu.virginia.cs.commandline.IsLeapYear 1900 --julian
# or
java edu.virginia.cs.commandline.IsLeapYear 1900 here are some unused arguments -j
# will output
1900 is a leap year
```

### Flag Argument Conventions
- optional arguments generally have 2 forms:
	1. short form: with 1 hyphen, ex. `-j`
	2. long form: with 2 hyphens, ex. `--julian`
### Optional Parameter Arguments
*example*

```shell
# using -c flag to specify any calendar
java edu.virginia.cs.commandline.IsLeapYear 1900 -c Julian
# Julian is the argument
```

## Tips for handling command line arguments
- provide useful error messages
- identify required and optional arguments
- use `List<String> argsList = Arrays.toList(args);`
- check all command line arguments before the rest of the program
- create a separate class to handle parsing arguments if the list is long