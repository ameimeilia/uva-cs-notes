---
layout: default
title: Construction cont.
parent: Midterm Notes
nav_order: 4
---
# Build Tools
## What build tools can do
1. simplifies the process of downloading and using external libraries
2. simplifies the process of updating to new libraries
3. simplifies the process of building a Java project into a `.jar` file
## Java Build Tools to know about
- **make**: `makefile` lists commands that are executed to generate a build file
- **ant**: uses XML configuration files to define the build process
- **maven**: also uses XML files, but more standardized, is also a project-management tool
- **gradle**: same features as maven but with shorter and easier to read syntax than XML
# Gradle
## The ease of gradle
- running `.\gradlew build` will:
	1. Download all the dependencies
	2. Compile all the code for you
	3. Run the existing test suite to make sure there are no known failures
	4. Build the output jar file for you to use
- automatically optimizes build process
## Gradle project structure
- **root directory**: project root folder
- **src folder**: contains all source code
- **gradle/wrapper**
- **build folder**: contains generated files when building the project (.class, .jar, test results, etc)
- **gradle files**: `build.gradle`, `settings.gradle`, `gradlew`, and `gradlew.bat` are files needed for gradle to function correctly
- `.gradle` folder: contains settings for gradle related to the `gradlew` command
## build.gradle
- build.gradle file is written in a **domain-specific language** using Groovy
### plugins

- core plugins describe the major aspects of the project to gradle

*example*

```Groovy
plugins {
	// project uses Java
    id 'java'
    // plugin for IntelliJ, not necessary
    id 'idea'
}
```

### Dependencies and Repository
- **repositories** section tells Gradle where to find libraries.

*example*

```Groovy
repositories {
	// most common Java library repository
	mavenCentral()
}
```
    
- **dependencies** lists the external libraries and their versions needed for the project

*example*

```Groovy
implementation group: 'org.json', name: 'json', version: '20220320'
```

- types of dependencies:
	1. `implementation`: required for compiling
	2. `runtimeOnly`: needed at runtime
	3. `testImplementation`: required for test code
	4. `testRuntimeOnly`: needed at test runtime
- to add a new dependency:
	1. search for the desired library in Maven Central Repository or MVNRepository
	2. add the Maven and Gradle code for the dependency
### test
- defines how to run tests
- running tests generates an HTML report located at `build/reports/tests/test/index.html`

*example*

```
test {
    useJUnitPlatform()
}
```

### jar
- tells Gradle how to build output .jar file

*example*

```
jar {
    archivesBaseName = "NBA-Excel"
    duplicatesStrategy = DuplicatesStrategy.EXCLUDE
    manifest {
        attributes "Main-class": "edu.virginia.cs.nbateams.Main"
    }

    from {
        configurations.runtimeClasspath.collect { it.isDirectory() ? it : zipTree(it) }
    }
}

group 'edu.virginia.cs.nbateams'
version '1.0'
```

- `archiveBaseName`: sets the output jar file name with the version number appended
- `duplicatesStrategy`: leave as `DuplicateStrategy.EXCLUDE` when making a “fat-jar”
- `manifest`: specifies the `Main-class` (main entry point) for a runnable jar
- `group`: used if you want to deploy .jar file on Maven, etc.
### fat-jar
- contains code and all dependencies

*example*

```
from {
    configurations.runtimeClasspath.collect { it.isDirectory() ? it : zipTree(it) }
}
```

## `gradle build`
- tells Gradle to execute the build script `build.gradle`, which ensures dependencies, compiles code, runs tests, and builds the jar file
## Gradle wrapper
- automatically installs correct Gradle for that project by running `./gradlew build`
- for Mac users: in the case of a “permission denied” error run `chmod +x gradlew`
### `.\gradlew build`
- limitation: if you want to create your own project as Gradle, you need Gradle installed
# Example Gradle with Poi
https://sde-coursepack.github.io/modules/construction/Example-Gradle-With-Poi/
## poi
- Apache Poi is a library for reading and writing Microsoft Office File Formats in Java
### org.apache.poi
Apache Poi libraries for reading Excel files:
1. `org.apache.poi.ss`: library for reading and writing an abstract SpreadSheer
2. `org.apache.poi.xssf`: library for reading and writing a `.xlsx` (Excel 2007 and later) spreadsheet
	- `xssf` stand for “XML Spreadsheet Format”
### Adding poi with Gradle
- add library to `build.gradle` file

```shell
dependencies {
	# add POI-OOXML library
    implementation group: 'org.apache.poi', name: 'poi-ooxml', version: '5.2.2'
    # add POI-OOXML dependency library log4j as runtimeOnly group
    runtimeOnly group: 'org.apache.logging.log4j', name: 'log4j-core', version: '2.18.0'

    testImplementation 'org.junit.jupiter:junit-jupiter-api:5.9.0'
    testRuntimeOnly 'org.junit.jupiter:junit-jupiter-engine:5.9.0'
}
```

### Implementation vs. runtime
