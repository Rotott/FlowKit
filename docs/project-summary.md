# Project Summary

This project is a lightweight, high-performance workflow execution engine written in modern C++20. It provides a generic framework for defining and executing tasks connected by dependencies in a Directed Acyclic Graph (DAG). Applications define their own task implementations, while FlowKit validates the graph structure, determines a valid execution order, and manages execution.

The project was created as a portfolio piece to bridge the gap between traditional enterprise object-oriented design and modern, low-overhead C++ paradigms. It demonstrates software architecture, graph algorithms, and clean system design developed during my Bachelor's degree in Systems Development at Malmö University.

### Architectural Philosophy
Rather than over-engineering the engine with heavy enterprise runtime abstractions, FlowKit focuses on C++ idiomatic practices:
- **Value Semantics & RAII:** Efficient memory management without relying on unnecessary heap allocation or pointer chasing.
- **Compile-Time Safety over Runtime Guessing:** Maximizing type safety and leveraging modern C++ structures to prevent runtime crashes.
- **Incremental Scaling:** Designed as a single-threaded Minimum Viable Product (MVP) with structural provisions to seamlessly transition to a multi-threaded execution pool.

[← Back to README](../README.md)