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
- [ ] **REQ-GRAPH-001:** The framework shall provide a programmatic API allowing users to define tasks as nodes and construct them into a Directed Acyclic Graph (DAG).
- [ ] **REQ-GRAPH-002:** The framework shall track and store explicit directional dependency edges between discrete task nodes.
- [ ] **REQ-GRAPH-003:** The framework shall validate the graph structure prior to execution using a single-pass implementation of **Kahn's algorithm**, rejecting any graph containing circular dependencies while simultaneously producing a valid topological execution order.
- [ ] **REQ-GRAPH-004:** The framework shall allow/validate disconnected subgraphs, ensuring isolated nodes are still scheduled during topological sort execution.
- [ ] **REQ-GRAPH-005:** The graph shall gracefully deduplicate duplicate directional edges between the same task pair
    - [ ] **REQ-GRAPH-005.1** The graph shall reject self-referencing edges (A -> A) during graph construction.
- [ ] **REQ-GRAPH-006:** Executing an empty graph shall either safely return a no-op success status or throw a invalid state error.
- [ ] **REQ-GRAPH-007:** The framework shall assign or accept a unique identifier (e.g., `TaskId` or `NodeId`) for each task node registered in the graph to uniquely specify dependency edges during graph construction.

### 1.2 Workflow execution
- [ ] **REQ-EXEC-001:** The framework shall use **Kahn's algorithm** to compute a valid topological execution order for all task nodes prior to workflow execution.
- [ ] **REQ-EXEC-002:** The executor shall enforce dependency order, ensuring no task node runs until all of its prerequisite dependency nodes have successfully completed.
- [ ] **REQ-EXEC-003:** The execution engine shall provide explicit runtime state feedback (e.g., Success or Failure) upon completion or halting of the workflow.
    - [ ] **REQ-EXEC-003.1:** The execution result payload shall return detailed summary data, including overall status (Success/Failure), total executed task count, failed task identifier (if any), and skipped task identifiers.
- [ ] **REQ-EXEC-004:** In the event of a task execution failure, the execution engine shall immediately halt the workflow and report a Failure state.
    - [ ] **REQ-EXEC-004.1:** Upon task failure, downstream tasks depending on the failed task must be explicitly marked as SKIPPED rather than left in an undefined pending state.
- [ ] **REQ-EXEC-005:** The execution engine shall execute tasks sequentially within a single thread of execution while remaining architecturally extensible for future parallel execution of independent nodes.
- [ ] **REQ-EXEC-006:** The executor shall support optional observer callbacks (or listener interfaces) for task state events (e.g., onTaskStart, onTaskSuccess, onTaskFailure).
- [ ] **REQ-EXEC-007:** The framework shall explicitly support reset mechanisms (or enforce single-use execution guarantees per graph instance) ensuring state from a prior run does not contaminate subsequent executions.

### 1.3 Task system
- [ ] **REQ-TASK-001:** The framework shall expose a structural contract (C++20 Concepts and/or template constraints) defining the execution contract for custom user tasks.
- [ ] **REQ-TASK-002:** Task-specific execution logic shall be strictly isolated, possessing no implicit knowledge of graph management, orchestration mechanics, or neighboring nodes.
- [ ] **REQ-TASK-003:** The framework shall support localized data passing by allowing the execution engine to safely route outputs from a completed task to the inputs of its downstream dependent tasks.
- [ ] **REQ-TASK-004:** Tasks shall accept an optional ExecutionContext or CancellationToken parameter during execute() to allow external interruption or cancellation.
- [ ] **REQ-TASK-005:** Task failures shall capture and expose exception messages or custom error payloads to the Executor for debugging.
- [ ] **REQ-TASK-006:** The task interface contract shall support move-only semantics, enabling tasks with non-copyable captures or members to be registered and executed cleanly.

### 1.4 Error handling & validation
- [ ] **REQ-ERR-001:** The framework shall throw a custom exception (e.g., `InvalidGraphException`) or return a structured error status when a circular dependency is detected during graph validation.
- [ ] **REQ-ERR-002:** The framework shall throw an exception or return a structured error status if a user attempts to add a dependency edge referencing a non-existent task node.
- [ ] **REQ-ERR-003:** The executor shall catch any uncaught exceptions thrown during task `execute()` calls, preventing process crashing, capturing the error message, and transitioning the task state to Failure.

### 1.5 Public API
- [ ] **REQ-API-001:** The framework shall expose a public API for constructing workflows without requiring users to directly manipulate graph storage containers.
- [ ] **REQ-API-002:** The public API shall support chained method calls for workflow construction.
- [ ] **REQ-API-003:** The public API shall permit task registration and dependency declaration in a readable and deterministic manner.

---

## 2. Non-functional requirements

### 2.1 Technical constraints & performance
- **REQ-PERF-001:** The framework shall minimize runtime overhead by leveraging compile-time polymorphism through C++20 templates and Concepts wherever appropriate, avoiding unnecessary virtual method dispatch (vtable lookups) and favoring efficient standard library facilities over heavyweight runtime design pattern abstractions.
- **REQ-PERF-002:** Graph validation and topological sorting shall complete in O(V + E) time, where V is the number of tasks and E is the number of dependency edges.
- **REQ-LANG-001:** The codebase shall strictly target the C++20 language standard and use CMake as its build system.
- **REQ-MEM-001:** The framework shall strictly adhere to RAII principles for resource management, ensuring no memory leaks occur during graph construction, validation, or execution.
- **REQ-MEM-002:** WorkflowGraph shall take full, value-based ownership or unique reference ownership (std::unique_ptr / move-only callables) of registered task nodes to prevent dangling references during graph destruction.

### 2.2 Architecture & maintainability
- **REQ-ARCH-001:** The framework shall separate graph storage, graph validation, and workflow execution into distinct components with clearly defined responsibilities.

### 2.3 Testability
- **REQ-TEST-001:** The system architecture shall accommodate comprehensive, automated unit testing for all graph operations, validation routines, and execution states via GoogleTest.

[← Back to README](../README.md)
