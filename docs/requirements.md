
# Requirements
**Note this file is just a placeholder for file structure, and needs an upd with the correct info.**

[← Back to README](../README.md)

## Project Goals

The primary goal of FlowKit is to demonstrate the design and implementation of a reusable workflow execution framework using modern C++20.

The project focuses on:

- Clean software architecture
- Separation of responsibilities
- Graph-based workflow execution
- Modern C++ development practices
- Test-driven development
- Maintainable and extensible code design

---

# Functional Requirements

## Workflow Definition

The framework shall:

- Allow users to create and configure workflows
- Allow custom task implementations through a common interface
- Allow tasks to be connected through dependencies
- Provide an API for defining workflow structure

---

## Workflow Graph Management

The framework shall:

- Represent workflows as a Directed Acyclic Graph (DAG)
- Store workflow nodes and their relationships
- Support dependency tracking between nodes
- Validate workflow structure before execution
- Detect circular dependencies

---

## Workflow Execution

The framework shall:

- Execute tasks according to dependency order
- Determine a valid execution sequence using graph algorithms
- Prevent execution of tasks whose dependencies have not completed
- Coordinate execution through a dedicated execution component

---

## Task System

The framework shall:

- Provide a base interface for executable workflow nodes
- Allow users to create custom task implementations
- Keep task logic independent from workflow orchestration

---

## Execution Context

The framework shall:

- Provide shared runtime data between workflow nodes
- Allow tasks to exchange information during execution
- Maintain workflow execution state

---

# Non-Functional Requirements

## Architecture

The system should:

- Follow SOLID principles
- Maintain clear separation between components
- Favor loose coupling and high cohesion
- Use abstractions where appropriate

---

## Maintainability

The system should:

- Have readable and well-structured code
- Use consistent coding standards
- Be easy to extend and refactor

---

## Testability

The system should:

- Support isolated unit testing of components
- Be developed using Test-Driven Development (TDD)
- Minimize unnecessary dependencies between modules

---

## Performance

The initial implementation should:

- Prioritize correctness and maintainability over optimization
- Avoid premature optimization
- Provide a foundation that can support future improvements

---

## Technical Constraints

The project shall:

- Use modern C++20 features
- Use CMake as the build system
- Support automated testing
- Use version control with Git

[← Back to README](../README.md)