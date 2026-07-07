# FlowKit
![CI Status](https://github.com/Rotott/FlowKit/actions/workflows/ci.yml/badge.svg)
![C++20](https://img.shields.io/badge/C%2B%2B-20-brightgreen)
![CMake](https://img.shields.io/badge/CMake-3.20%2B-brightgreen)
![Branch Coverage](https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/Rotott/FlowKit/badges/branch-coverage.json)
A modern, low-overhead C++20 workflow engine for building dependency-driven task pipelines. 

FlowKit provides a clean framework for defining tasks and connecting them into a Directed Acyclic Graph (DAG). The framework automatically handles dependency validation, cycle detection, and topological execution ordering, keeping orchestration completely separate from individual task logic.

---

## Features
- **Fluent Graph API:** Easily declare tasks and define dependency structures.
- **Strict DAG Validation:** Automated runtime validation and cycle detection to prevent broken pipelines.
- **Topological Execution:** Resolves and runs tasks sequentially based on mathematical dependency order.
- **Modern C++ Core:** Employs RAII, clean type abstractions, and standard library graph algorithms.

---

## Architecture & Roadmap
FlowKit is explicitly designed around separation of concerns to support clean software engineering metrics. It is currently implemented as a highly deterministic, single-threaded execution engine (MVP). The clean separation between the `WorkflowGraph` and the `Executor` ensures that a concurrent, thread-pool-based executor can be integrated in the future without modifying user-defined tasks.

For a deep dive into design choices, see the links below:
- [Project Summary](docs/project-summary.md)
- [Requirements Specification](docs/requirements.md)
- [Architecture & Components](docs/architecture.md)
- [Development & Quality Goals](docs/development.md)

---

## Tech Stack
- **Language:** C++20
- **Build System:** CMake
- **Testing:** GoogleTest 
- **Documentation Diagrams:** Mermaid

---

## Example Usage **(TBD)**

```cpp
#include <flowkit/FlowKit.hpp>
#include <iostream>

// Define a custom task by inheriting from the base Task class
class DownloadDataTask : public flowkit::Task {
public:
    bool execute() override {
        std::cout << "Downloading raw assets...\n";
        return true; // Return success
    }
};

int main() {
    flowkit::WorkflowGraph graph;

    // Register tasks
    graph.add_task("download", std::make_unique<DownloadDataTask>());
    graph.add_task("process", []() { 
        std::cout << "Processing assets...\n"; 
        return true; 
    });

    // Define dependencies (Process depends on Download)
    graph.add_dependency("download", "process");

    // Execute the workflow
    flowkit::Executor executor;
    bool success = executor.run(graph);

    return success ? 0 : 1;
}
```

**//TODO add context changing between nodes**