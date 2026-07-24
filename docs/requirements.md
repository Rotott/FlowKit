# FlowKit requirements specification

## Requirement tag legend
Before reviewing the specifications, use this table to cross-reference the requirement identifiers:

| Tag Prefix | Category | Description |
| :--- | :--- | :--- |
| **REQ-GRAPH** | Functional | Graph architecture, building, and structural data rules. |
| **REQ-EXEC** | Functional | Execution workflow, ordering, and sequencing rules. |
| **REQ-TASK** | Functional | Task interface specifications and isolation rules. |
| **REQ-ERR** | Functional | Validation failures and exception handling rules. |
| **REQ-API** | Functional | Public API design, usability, and workflow construction rules.
| **REQ-PERF** | Non-Functional | Runtime overhead limitations and performance choices. |
| **REQ-LANG** | Non-Functional | Language standard and build system requirements. |
| **REQ-MEM** | Non-Functional | Memory management patterns (e.g., RAII). |
| **REQ-ARCH** | Non-Functional | Structural cohesion and architecture bounds. |
| **REQ-TEST** | Non-Functional | Automated unit testing requirements. |

---

## Table of Contents
* [1. Functional requirements](#1-functional-requirements)
    * [1.1 Workflow definition & graph management](#11-workflow-definition--graph-management)
    * [1.2 Workflow execution](#12-workflow-execution)
    * [1.3 Task system](#13-task-system)
    * [1.4 Error handling & validation](#14-error-handling--validation)
    * [1.5 Public API](#15-public-api)
* [2. Non-functional requirements](#2-non-functional-requirements)
    * [2.1 Technical constraints & performance](#21-technical-constraints--performance)
    * [2.2 Architecture & maintainability](#22-architecture--maintainability)
    * [2.3 Testability](#23-testability)

---

## 1. Functional requirements

### 1.1 Workflow definition & graph management
- [ ] **REQ-GRAPH-001:** The framework shall provide a programmatic builder API to construct a Directed Acyclic Graph (DAG) of task nodes.
- [ ] **REQ-GRAPH-002:** The framework shall track directional dependency edges between unique task nodes (`A -> B` where A runs before B).
- [ ] **REQ-GRAPH-003:** The framework shall validate the graph using **Kahn's algorithm**, throwing an `InvalidGraphException` if a cycle is detected, or producing a valid topological ordering on success.
- [ ] **REQ-GRAPH-004:** Disconnected subgraphs and isolated nodes shall be validated and included in the final topological execution order.
- [ ] **REQ-GRAPH-005:** Adding a duplicate directional edge between two already connected nodes shall act as a no-op without duplicating edge records.
    - [ ] **REQ-GRAPH-005.1:** Adding a self-referencing edge (`A -> A`) shall throw an `InvalidGraphException`.
- [ ] **REQ-GRAPH-006:** Executing an empty graph shall return a `Success` state with an executed task count of 0.
- [ ] **REQ-GRAPH-007:** Each task node registered in the graph shall be assigned or provided a unique `TaskId`.

### 1.2 Workflow execution
- [ ] **REQ-EXEC-001:** The executor shall execute tasks strictly according to the topological ordering generated during graph validation.
- [ ] **REQ-EXEC-002:** Downstream tasks shall not begin execution until all prerequisite upstream tasks report `Success`.
- [ ] **REQ-EXEC-003:** Workflow execution shall return an `ExecutionResult` struct containing overall status (`Success`/`Failure`), total executed count, failed task ID (if failed), and a list of skipped task IDs.
- [ ] **REQ-EXEC-004:** On task failure, execution shall immediately halt without attempting remaining independent branches.
    - [ ] **REQ-EXEC-004.1:** All unexecuted tasks dependent on a failed task shall be marked as `SKIPPED` in the `ExecutionResult`.
- [ ] **REQ-EXEC-005:** Tasks shall execute sequentially on the calling thread.
- [ ] **REQ-EXEC-006:** The executor shall accept task lifecycle listeners with callbacks for `onTaskStart`, `onTaskSuccess`, and `onTaskFailure`.
- [ ] **REQ-EXEC-007:** Executing a workflow shall reset internal node runtime states, allowing the same graph instance to be re-executed cleanly.
### 1.3 Task system
- [ ] **REQ-TASK-001:** The framework shall expose a structural contract (C++20 Concepts and/or template constraints) defining the execution contract for custom user tasks.
- [ ] **REQ-TASK-002:** Task-specific execution logic shall be strictly isolated, possessing no implicit knowledge of graph management, orchestration mechanics, or neighboring nodes.
- [ ] **REQ-TASK-003:** The framework shall support localized data passing by allowing the execution engine to safely route outputs from a completed task to the inputs of its downstream dependent tasks.
- [ ] **REQ-TASK-004:** Tasks shall accept an optional ExecutionContext or CancellationToken parameter during execute() to allow external interruption or cancellation.
- [ ] **REQ-TASK-005:** Task failures shall capture and expose exception messages or custom error payloads to the Executor for debugging.
- [ ] **REQ-TASK-006:** The task interface contract shall support move-only semantics, enabling tasks with non-copyable captures or members to be registered and executed cleanly.

### 1.4 Error handling & validation
- [ ] **REQ-ERR-001:** The framework shall throw a custom exception (e.g., `InvalidGraphException`) or return a structured error status when a circular dependency is detected during graph validation.
- [ ] **REQ-ERR-002:** Attempting to add a dependency edge referencing a non-existent `TaskId` shall throw an `InvalidGraphException`.
- [ ] **REQ-ERR-003:** The executor shall catch any uncaught exceptions thrown during task `execute()` calls, preventing process crashing, capturing the error message, and transitioning the task state to Failure.

### 1.5 Public API
- [ ] **REQ-API-001:** The framework shall expose a public API for constructing workflows without requiring users to directly manipulate graph storage containers.
- [ ] **REQ-API-002:** The public API shall support chained method calls for workflow construction.
- [ ] **REQ-API-003:** The public API shall permit task registration and dependency declaration in a readable and deterministic manner.

---

## 2. Non-functional requirements

### 2.1 Technical constraints & performance
- **REQ-PERF-001:** The framework shall use C++20 Concepts to constrain user task types at registration time (`addTask<T>()`), while using type erasure (e.g., `std::function` or light interfaces) for heterogeneous graph node storage.
- **REQ-PERF-002:** Graph validation and topological sorting shall complete in O(V + E) time, where V is the number of tasks and E is the number of dependency edges.
- **REQ-LANG-001:** The codebase shall strictly target the C++20 language standard and use CMake as its build system.
- **REQ-MEM-001:** The framework shall strictly adhere to RAII principles for resource management, ensuring no memory leaks occur during graph construction, validation, or execution.
- **REQ-MEM-002:** WorkflowGraph shall take full, value-based ownership or unique reference ownership (std::unique_ptr / move-only callables) of registered task nodes to prevent dangling references during graph destruction.

### 2.2 Architecture & maintainability
- **REQ-ARCH-001:** The framework shall separate graph storage, graph validation, and workflow execution into distinct components with clearly defined responsibilities.

### 2.3 Testability
- **REQ-TEST-001:** The system architecture shall accommodate comprehensive, automated unit testing for all graph operations, validation routines, and execution states via GoogleTest.

[← Back to README](../README.md)
