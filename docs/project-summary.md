# Project summary & design intentions

This project is a lightweight, high-performance workflow execution engine written in modern C++20. It provides a generic framework for defining and executing tasks connected by dependencies in a Directed Acyclic Graph (DAG).

FlowKit was created as a portfolio piece to bridge the gap between traditional enterprise object-oriented design and modern, low-overhead C++ paradigms. It demonstrates software architecture, graph algorithms, and clean system design developed during my Bachelor's degree in Systems Development at Malmö University.

---

## Architectural philosophy & intentions

Rather than over-engineering the engine with heavy enterprise runtime abstractions, FlowKit focuses on idiomatic C++ practices and clear system goals:

### 1. High cohesion & separation of concerns
- **Intent:** Keep graph storage, validation, and execution mechanisms strictly separated to maintain a modular architecture.
- **Rationale:** Separating the structural model from the execution logic ensures the codebase remains maintainable, testable, and easy to reason about. Graph validation is performed using an efficient single-pass implementation of **Kahn's algorithm**, which simultaneously detects cycles and establishes a valid execution order for the workflow.

### 2. Value semantics & low-overhead RAII
- **Intent:** Leverage modern C++20 features (e.g., standard library concepts, improved containers) and strict RAII.
- **Rationale:** Efficient memory management without relying on unnecessary heap allocation, pointer chasing, or heavy design pattern abstractions where standard library mechanisms suffice.

### 3. Compile-time safety over runtime guessing
- **Intent:** Maximize type safety through modern C++20 features, including templates and concepts, while favoring compile-time polymorphism over runtime inheritance where appropriate.
- **Rationale:** Architectural constraints and interface requirements are enforced during compilation, reducing runtime errors. By relying on compile-time polymorphism instead of pervasive virtual dispatch, the framework avoids much of the runtime overhead traditionally associated with enterprise object-oriented designs while remaining flexible and extensible.

### 4. TDD foundation & incremental scaling
- **Intent:** Build the framework using Test-Driven Development (TDD) as a single-threaded Minimum Viable Product (MVP).
- **Rationale:** Ensures strict code reliability from the ground up, providing the structural guarantees and high unit test coverage needed to seamlessly transition to a multi-threaded execution pool later.

[← Back to README](../README.md)
