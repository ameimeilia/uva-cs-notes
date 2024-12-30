---
layout: default
title: Architecture
parent: Final Notes
nav_order: 1
---
# Architecture
- Refers to the design of the overall project
- Focuses on **components**, rather than individual classes/methods or modules
## Components
- **Components**: Major sub-systems in the software system
- Can include:
    - Groups of modules handling a feature/feature set
    - Independent programs (external)
    - Data sources (databases, files, web services)
    - External connections (servers, browsers)
- Modularity is broader here compared to individual modules
## Architectural Concerns
- Boundaries of components for communication
- Separation of major concerns in the system
- Long-term maintainability
### Boundaries
- Software interacts with external systems (e.g., APIs, libraries)
	- Brittle code if API structure changes or service goes offline
- Set up systems to allow replacement of external components
	- Use abstraction (e.g., interfaces, abstract classes)
	- Hide complexity behind abstraction to prepare for changes
	- Implement **Adaptor** design pattern for third-party services
### Separation of Concerns
- Ensure components handle specific concerns (modularity at a higher level)
- Clear separation of responsibilities between components
### Long Term Maintainability
- Architecture should be clear, understandable, and cohesive
- Components should be functionally independent
- Boundaries should abstract complexities
- Poor architectural decisions lead to software entropy, making maintenance difficult
- **Architectural changes are the hardest to make**
# Architectural Patterns
## What is a Pattern
- A **pattern** is an abstract strategy for addressing design concerns.
- Patterns are not directly implementable like code or frameworks; instead, they are general ideas for solving design problems.
## Pattern vs Framework
- **Framework**: Pre-built code/structure to help build applications (e.g., Java Collections Framework, JavaFX).
- **Framework** provides specific functionalities (like `List`, `Set`, `Map`) for developers to build on, while patterns guide how to structure systems.
## Common Architectural Patterns
### Big Ball of Mud - avoid at all costs!
- Represents a system without clear, disciplined architecture.
- Components interact ad-hoc, leading to complexity and breakdown of modularity and functional independence.
- Can arise even in systems that initially had a structured design.

![img.png | 500](https://sde-coursepack.github.io/modules/patterns/images/arch_patterns/big_ball_of_mud.png)

### Layered Architecture
- Components are organized in layers, each interacting only with its adjacent layers (above and below).
- Layers provide services to the layer above and send requests to the layer below.
- Helps visualize systems from high-level to low-level, enhancing modularity and functional independence.

![img.png](https://sde-coursepack.github.io/modules/patterns/images/arch_patterns/reference_architecture.png)
### Networked Architectures
#### Client Server
- Two components: client and server, communicating over the internet.
- Client (e.g., a mobile app) sends requests to the server, which handles data and sends responses.
- Issues include server load, synchronization, and handling outages.
#### Peer-to-Peer
- No centralized server; clients communicate directly with each other.
- Often used in file-sharing and high-end computing to distribute workload.
- Security concerns must be considered.
### Monolithic Architecture
- The entire application is deployed as a single unit, often on a server.
- Advantages: easy scalability and updates.
- Drawbacks: becomes harder to maintain as the application grows, and it’s difficult to introduce new technologies.
### Microservice Architecture
- Application is broken into small, independent services (microservices), each with its own API.
- Each microservice can be maintained and deployed independently.
- Benefits: modularity, ease of maintenance.
- Trade-offs: managing multiple services increases complexity, potential bandwidth and memory issues.
# Three Layer Architecture
- A Three Layer Architecture is a common example of a **Layered Architecture**.
- Three separate component layers:
	  1. **Presentation**: user interface
	  2. **Business Logic**: analyzing the data and applying real-world rules to produce some insight, summary, or modification of the data
	  3. **Data**: manages the data to be interpreted by higher layers
- Each component can be managed independent of others as long as the expected interface between layers in maintained
## Portability
A significant advantage of the Three Layer Architecture is **portability**.
- Changes can be made to one layer (e.g., Presentation) without affecting others (e.g., Business Logic, Data).
- This allows flexibility in adding features or modifying components as the application evolves.