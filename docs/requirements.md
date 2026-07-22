# FlowKit requirements specification

## Requirement tag legend
Before reviewing the specifications, use this table to cross-reference the requirement identifiers:

| Tag Prefix | Category | Description |
| :--- | :--- | :--- |
| **REQ-GRAPH** | Functional | Graph architecture, building, and structural data rules. |
| **REQ-EXEC** | Functional | Execution workflow, ordering, and sequencing rules. |
| **REQ-TASK** | Functional | Task interface specifications and isolation rules. |
| **REQ-ERR**  | Functional | Validation failures and exception handling rules. |
| **REQ-PERF** | Non-Functional | Runtime overhead limitations and performance choices. |
| **REQ-LANG** | Non-Functional | Language standard and build system requirements. |
| **REQ-MEM**  | Non-Functional | Memory management patterns (e.g., RAII). |
| **REQ-ARCH** | Non-Functional | Structural cohesion and architecture bounds. |
| **REQ-TEST** | Non-Functional | Automated unit testing requirements. |

---
## Table of Contents
* [1. Functional requirements](#1-functional-requirements)
    * [1.1 Workflow definition & graph management](#11-workflow-definition--graph-management)
    * [1.2 Workflow execution](#12-workflow-execution)
    * [1.3 Task system](#13-task-system)
    * [1.4 Error handling & validation](#14-error-handling--validation)
* [2. Non-functional requirements](#2-non-functional-requirements)
    * [2.1 Technical constraints & performance](#21-technical-constraints--performance)
    * [2.2 Architecture & maintainability](#22-architecture--maintainability)
    * [2.3 Testability](#23-testability)

---

## 1. Functional requirements

### 1.1 Workflow definition & graph management
* - [ ] **REQ-GRAPH-001:** The framework shall provide a programmatic API allowing users to define tasks as nodes and construct them into a Directed Acyclic Graph (DAG).
* - [ ] **REQ-GRAPH-002:** The framework shall track and store explicit directional dependency edges between discrete task nodes.
* - [ ] **REQ-GRAPH-003:** The framework shall validate the graph structure prior to execution and explicitly reject any graph containing circular dependencies.

### 1.2 Workflow execution
* - [ ] **REQ-EXEC-001:** The framework shall execute a topological sort algorithm to resolve a valid sequential execution order for the graph.
* - [ ] **REQ-EXEC-002:** The executor shall enforce dependency order, ensuring no task node runs until all of its prerequisite dependency nodes have successfully completed.
* - [ ] **REQ-EXEC-003:** The execution engine shall provide explicit runtime state feedback (e.g., Success or Failure) upon completion or halting of the workflow.
* - [ ] **REQ-EXEC-004:** In the event of a task execution failure, the execution engine shall immediately halt the workflow and report a Failure state.
* - [ ] **REQ-EXEC-005:** The execution engine shall execute tasks sequentially within a single thread of execution (shall support future updates for parallel execution of independent nodes).
	
### 1.3 Task system
* - [ ] **REQ-TASK-001:** The framework shall expose a distinct interface or base-class defining the execution contract for custom user tasks.
* - [ ] **REQ-TASK-002:** Task-specific execution logic shall be strictly isolated, possessing no implicit knowledge of graph management, orchestration mechanics, or neighboring nodes.
* - [ ] **REQ-TASK-003:** The framework shall support localized data passing by allowing tasks to yield outputs that the execution engine maps to the inputs of downstream dependent tasks.

### 1.4 Error handling & validation
* - [ ] **REQ-ERR-001:** The framework shall throw a custom exception (e.g., `InvalidGraphException`) or return a structured error status when a circular dependency is detected during validation. 
---

## 2. Non-functional requirements

### 2.1 Technical constraints & performance
* **REQ-PERF-001:** The framework shall minimize standard runtime overhead by utilizing C++ standard library mechanisms (`std::function`, standard algorithms) instead of heavy design pattern abstractions where applicable.
* **REQ-LANG-001:** The codebase shall strictly target the C++20 language standard and  use CMake as its build system.
* **REQ-MEM-001:** The framework shall strictly adhere to RAII principles for resource management, ensuring no memory leaks occur during graph construction, validation, or execution.


### 2.2 Architecture & maintainability
* **REQ-ARCH-001:** The framework shall separate graph storage, graph validation, and workflow execution into distinct components with clearly defined responsibilities.

### 2.3 Testability
* **REQ-TEST-001:** The system architecture shall accommodate comprehensive, automated unit testing for all graph operations, validation routines, and execution states via GoogleTest.

[← Back to README](../README.md)



