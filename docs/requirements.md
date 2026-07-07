# Requirements

## Project Goals
The primary goal of FlowKit is to showcase modern C++20 development practices through a clean, highly cohesive workflow engine.

---

# Functional Requirements

## Workflow Definition & Graph Management
The framework shall:
- Allow users to define tasks (as nodes) and construct them into workflows as Directed Acyclic Graphs (DAGs) using a clean programmatic API.
- Validate the graph structure prior to execution, explicitly detecting and rejecting circular dependencies.
- Track structural dependencies between discrete task nodes.

## Workflow Execution
The framework shall:
- Perform a topological sort to resolve a valid sequential execution order for the graph.
- Execute tasks strictly according to dependency order, ensuring no task runs until its dependencies complete.
- Provide runtime state feedback (e.g., Success, Failure) upon completion of execution.

## Task System
The framework shall:
- Provide a clear interface/base-class for custom executable tasks.
- Isolate task-specific execution logic entirely from graph management and orchestration logic.

---

# Non-Functional Requirements

## Architecture & Maintainability
- **High Cohesion:** Keep graph storage, validation, and execution mechanisms strictly separated.
- **Low Overhead:** Avoid unnecessary virtual method dispatch or heavy design pattern abstractions where standard library mechanisms (`std::function`, algorithms) suffice.
- **Modern C++20 Constraints:** Leverage C++20 features (e.g., standard library concepts, improved containers) using CMake as the build system.

## Testability
- Accommodate comprehensive unit testing via GoogleTest.
- Built using Test-Driven Development (TDD) to ensure code reliability and structural flexibility for future performance optimizations.

[← Back to README](../README.md)